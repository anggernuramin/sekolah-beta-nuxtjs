<template>
  <header
    class="fixed z-50 lg:w-[100%] md:w-[100%] sm:w-[100%] bg-primary text-slate-50"
  >
    <div class="container py-3 lg:w-[90%] flex justify-between items-center">
      <div class="logo">
        <h1 class="text-slate-50 text-3xl font-bold">
          ANA<span class="text-btnColor">SHOES</span>
        </h1>
      </div>

      <div class="header-icon flex gap-5 justify-center items-center">
        <div v-if="user" class="flex gap-5">
          <nuxt-link to="/wishlist" class="cursor-pointer text-base">
            <div
              class="indicator tooltip tooltip-bottom tooltip-warning"
              data-tip="wislist saya"
            >
              <span class="indicator-item badge badge-secondary">{{
                notifWishListItems.length
              }}</span>

              <div
                class="grid w-6 h-6 bg-base-300 place-items-center bg-transparent"
              >
                <i
                  class="fa-regular fa-heart text-lg text-white bg-transparent"
                ></i>
              </div>
            </div>
            Wishlist
          </nuxt-link>

          <nuxt-link to="/keranjang" class="cursor-pointer text-base">
            <div
              class="indicator tooltip tooltip-bottom tooltip-warning cursor-pointer"
              data-tip="keranjang saya"
            >
              <span class="indicator-item badge badge-secondary">{{
                notifListItemsKeranjang.length
              }}</span>
              <div
                class="grid w-6 h-6 bg-base-300 place-items-center bg-transparent"
              >
                <i
                  class="fas fa-shopping-cart text-lg text-white bg-transparent"
                ></i>
              </div>
            </div>
            Cart
          </nuxt-link>
          <div class="dropdown dropdown-hover dropdown-end">
            <i class="fas fa-user"></i>

            <label tabindex="0" class="m-1">Profile</label>
            <ul
              tabindex="0"
              class="dropdown-content z-[1] menu p-2 shadow bg-white text-slate-900 rounded-box w-52"
            >
              <li><nuxt-link to="/profile">Profile saya</nuxt-link></li>
              <li><nuxt-link to="/alamat">Alamat saya</nuxt-link></li>
              <li><button @click="logOut">Log out</button></li>
            </ul>
          </div>
        </div>
        <div v-else>
          <button
            class="ms-2 border-2 border-transparent outline-none bg-btnColor py-2 px-8 text-base cursor-pointer transition font-bold rounded-sm hover:bg-transparent hover:border-btnColor"
            type="button"
          >
            <nuxt-link to="/login">Login</nuxt-link>
          </button>
          <button
            id="btn-register"
            class="ms-2 border-2 border-btnColor outline-none bg-transparent py-2 px-8 text-base cursor-pointer transition font-bold rounded-sm hover:bg-btnColor hover:border-transparent"
            type="button"
          >
            <nuxt-link to="/register">Register</nuxt-link>
          </button>
        </div>
      </div>
      <div class="burger-menu lg:hidden md:hidden sm:hidden">
        <i id="menu-hamburger" class="fa-solid fa-bars"></i>
      </div>
    </div>
    <nav
      class="wrapper-nav-list container lg:w-[90%] border-t-[1px] border-r-gray-50 py-3"
    >
      <ul class="flex justify-start items-center">
        <li class="mr-4">
          <nuxt-link
            to="/"
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            >Home</nuxt-link
          >
        </li>
        <li class="mr-4">
          <nuxt-link
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            to="/about"
            >About</nuxt-link
          >
        </li>
        <li class="mr-4">
          <nuxt-link
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            to="/products"
            >Products</nuxt-link
          >
        </li>
        <li class="mr-4">
          <nuxt-link
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            to="/newArrival"
            >New Arrival</nuxt-link
          >
        </li>
        <li class="mr-4">
          <nuxt-link
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            to="/reviews"
            >Reviews</nuxt-link
          >
        </li>
        <li class="mr-4">
          <nuxt-link
            class="relative after:absolute after:block after:content-'' after:bg-white after:h-[3px] after:w-1/2 after:opacity-0 after:transition-opacity after:group-hover:opacity-100 after:left-1/2 after:translate-x-[-50%]"
            to="/contact"
            >Contact</nuxt-link
          >
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      user: null,
    }
  },
  computed: {
    // penamaan computed tidak karus sama dengan state yg ada di store, begitupun juga dengan method
    notifWishListItems() {
      // this.$store.state.namaFolder.namaStateyangAkanDiPanggil
      return this.$store.state.index.wishListItems
    },
    notifListItemsKeranjang() {
      return this.$store.state.index.listDataBelanja
    },
  },
  async created() {
    try {
      const {
        data: { user },
      } = await this.$supabase.auth.getUser()
      this.user = user
    } catch (error) {}
  },
  // vue hook mounted(dom sudah dirender dengan baik)
  mounted() {
    const burgerMenu = document.getElementById('menu-hamburger')
    const menuList = document.querySelector('.wrapper-nav-list')
    burgerMenu.addEventListener('click', () => {
      menuList.classList.toggle('toggle-nav-active')
    })
  },
  methods: {
    async logOut() {
      try {
        const { error } = await this.$supabase.auth.signOut()
        if (error) {
          throw error
        }
        this.$router.push('/login')
      } catch (error) {}
    },
  },
}
</script>

<style>
/* media query mobile */
@media screen and (max-width: 675px) {
  header {
    width: 100%;
    padding: 10px;
  }
  header .container {
    flex-direction: column;
    gap: 10px;
    justify-content: center;
  }
  .header-icon {
    margin-left: 15px;
  }
  #btn-register {
    margin-right: 50px;
  }
  .header-icon button {
    padding-block: 5px;
    padding-inline: 12px;
  }
  header .burger-menu {
    position: absolute;
    color: white;
    display: block;
    top: 23px;
    right: 25px;
    font-size: 24px;
    z-index: 9999;
  }
  .wrapper-nav-list {
    display: none;
    padding: 0;
    transition: 9s;
    z-index: 9999;
  }
  .wrapper-nav-list.toggle-nav-active {
    display: block;
  }
  .wrapper-nav-list > ul {
    position: absolute;
    z-index: 9999;
    right: 0;
    flex-direction: column;
    align-items: flex-start;
    width: 80%;
    height: 100vh;
    background-color: #112852;
    padding: 20px;
  }
  .wrapper-nav-list > ul > li {
    margin-right: 0px;
    margin-top: 20px;
    font-size: 22px;
    width: 100%;
    padding: 10px;
    color: white;
  }

  .wrapper-nav-list > ul > li:nth-child(1) {
    margin-top: 0px;
  }
  .header-logo {
    width: 100%;
  }
  .wrapper-nav-list > ul > li > a::after {
    top: 30px;
    right: 0%;
    transform: translateX(-50%);
  }
}
</style>
