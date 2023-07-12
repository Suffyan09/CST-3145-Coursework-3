<template>
  <div class="container backpic p-5" style="margin-top: 50px">
    <main>
        <!-- <div class="col-md-7 ">
          <img src="../assets/background.jpg">
        </div> -->

        <div class="">
          <h2 class="mb-3 text-white">Checkout Form</h2>
          <form class="needs-validation">
            <div class="row g-3">
              <div class="col-12">
                <input
                  type="text"
                  v-model="user.name"
                  class="form-control"
                  id="firstName"
                  placeholder="Name on ID"
                />

              </div>

              <div class="col-12">
                <input
                  type="email"
                  class="form-control"
                  id="email"
                  v-model="user.email"
                  placeholder="you@example.com"
                />
              </div>

            </div>

            <hr class="my-4" />

            <button  class="w-100 btn btn-outline-dark btn-lg" value="Checkout" @click="submitCheckout"  style="margin-bottom: 25px">
              Checkout
            </button>
          </form>
        </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "checkOut",
  data() {
    return {
      cart: [], // Assuming you have a cart array with items
      user: {
        name: "",
        email: "",
        gift: false,
      },
    };
  },
  computed: {
    cartTotal() {
      return this.cart.reduce(
        (total, product) => total + product.price * product.space,
        0
      );
    },
    cartCount() {
      let count = 0;
      this.cart.forEach((item) => {
        count += item.space;
      });
      return count;
    },
  },
  methods: {
    navigateTo(page) {
      this.page = page;
    },
    submitCheckout() {
      // Update the space property of lessons in the cart
      this.cart.forEach((item) => {
        const lessonIndex = this.lessons.findIndex(
          (lesson) => lesson.id === item.id
        );
        if (lessonIndex !== -1) {
          this.lessons[lessonIndex].space += 1;
        }
      });

      // Create the order object with updated space property
      const order = {
        checkoutName: this.user.name,
        checkoutemail: this.user.email,
        checkoutaddress: this.user.address,
        checkoutstate: this.user.state,
        checkoutphone: this.user.phone,

        cartProduct: this.cart,
      };

      // Send the order to the server
      fetch("http://localhost:3000/collection/order", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        mode: "cors",
        cache: "no-store",
        body: JSON.stringify(order),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log("Order submitted:", data);
          alert(data.message);
          // Handle the response data here (e.g., show a success message)
        })
        .catch((error) => {
          console.log("Error submitting order:", error);
          // Handle the error here (e.g., show an error message)
        });

      // Reset the cart
      this.cart = [];
      // Reset the user details
      this.user = {
        name: "",
        email: "",
        gift: false,
      };
    },
  },
};
</script>

<style>
.backpic{
  background: #cb2d3e;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #ef473a, #cb2d3e);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #ef473a, #cb2d3e); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

}
</style>
