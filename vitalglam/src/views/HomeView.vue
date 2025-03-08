<template>
  <div id="app" class="min-h-screen flex flex-col">
    <!-- Header with Navigation -->
    <header class="bg-white shadow-sm sticky top-0 z-50">
      <div class="container mx-auto px-4">
        <nav class="flex items-center justify-between py-4">
          <div class="flex items-center space-x-2">
            <i class="fas fa-leaf text-green-500 text-2xl"></i>
            <span class="text-2xl font-bold text-gray-800">Vital<span class="text-green-500">Glam</span></span>
          </div>
          <div class="hidden md:flex space-x-8">
            <a v-for="item in navItems" :key="item.id" 
               :href="item.url" 
               class="text-gray-600 hover:text-green-500 transition-colors duration-200">
              {{ item.title }}
            </a>
          </div>
          <div class="flex items-center space-x-4">
            <button class="hidden md:block px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600 transition-colors duration-200">
              Subscribe
            </button>
            <!-- Mobile Menu Button -->
            <button @click="toggleMobileMenu" class="md:hidden text-gray-600">
              <i class="fas fa-bars text-xl"></i>
            </button>
          </div>
        </nav>
        <!-- Mobile Menu -->
        <div v-if="mobileMenuOpen" class="md:hidden py-4 border-t border-gray-100">
          <a v-for="item in navItems" :key="item.id" 
             :href="item.url" 
             class="block py-2 text-gray-600 hover:text-green-500 transition-colors duration-200">
            {{ item.title }}
          </a>
          <button class="mt-4 w-full px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600 transition-colors duration-200">
            Subscribe
          </button>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-green-50 to-green-100 py-12 md:py-20">
      <div class="container mx-auto px-4">
        <div class="flex flex-col md:flex-row items-center">
          <div class="md:w-1/2 mb-8 md:mb-0">
            <h1 class="text-4xl md:text-5xl font-bold text-gray-800 mb-4 animate-fade-in">
              Your Journey to Wellness Starts Here
            </h1>
            <p class="text-lg text-gray-600 mb-6 animate-fade-in delay-100">
              Discover the latest in health trends, nutrition advice, and premium wellness products that transform lives.
            </p>
            <div class="flex space-x-4 animate-fade-in delay-200">
              <button class="px-6 py-3 bg-green-500 text-white rounded-md hover:bg-green-600 transition-colors duration-200">
                Read Latest
              </button>
              <button class="px-6 py-3 border border-green-500 text-green-500 rounded-md hover:bg-green-50 transition-colors duration-200">
                Shop Products
              </button>
            </div>
          </div>
          <div class="md:w-1/2 animate-fade-in delay-300">
            <img src="/health.jpg" alt="Wellness lifestyle" class="rounded-lg shadow-md transform hover:scale-105 transition-transform duration-300">
          </div>
        </div>
      </div>
    </section>

    <!-- Featured Posts -->
    <section class="py-12 bg-white">
      <div class="container mx-auto px-4">
        <div class="flex justify-between items-center mb-8">
          <h2 class="text-2xl md:text-3xl font-bold text-gray-800">Featured Articles</h2>
          <a href="#" class="text-green-500 hover:text-green-600 flex items-center">
            View All <i class="fas fa-arrow-right ml-2"></i>
          </a>
        </div>
        
        <div v-if="loadingPosts" class="text-center py-8">
          <i class="fas fa-spinner fa-spin text-green-500 text-2xl"></i>
          <p class="text-gray-600 mt-2">Loading posts...</p>
        </div>
        <div v-else-if="errorPosts" class="text-center py-8 text-red-500">
          {{ errorPosts }}
        </div>
        <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div v-for="post in featuredPosts" :key="post.id" class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow duration-200">
            <img :src="post.image" :alt="post.title" class="w-full h-48 object-cover">
            <div class="p-6">
              <div class="flex items-center mb-4">
                <span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded-full">{{ post.category }}</span>
                <span class="text-gray-500 text-sm ml-2">{{ post.date }}</span>
              </div>
              <h3 class="text-xl font-semibold text-gray-800 mb-2">{{ post.title }}</h3>
              <p class="text-gray-600 mb-4">{{ post.excerpt }}</p>
              <a href="#" class="text-green-500 hover:text-green-600 font-medium">Read more</a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Product Promotion -->
    <section class="py-12 bg-gray-50">
      <div class="container mx-auto px-4">
        <h2 class="text-2xl md:text-3xl font-bold text-gray-800 text-center mb-4">Featured Products</h2>
        <p class="text-gray-600 text-center max-w-2xl mx-auto mb-12">Discover our curated selection of premium wellness products that support your health journey.</p>
        
        <div v-if="loadingProducts" class="text-center py-8">
          <i class="fas fa-spinner fa-spin text-green-500 text-2xl"></i>
          <p class="text-gray-600 mt-2">Loading products...</p>
        </div>
        <div v-else-if="errorProducts" class="text-center py-8 text-red-500">
          {{ errorProducts }}
        </div>
        <div v-else class="grid grid-cols-1 md:grid-cols-4 gap-6">
          <div v-for="product in featuredProducts" :key="product.id" class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow duration-200">
            <div class="relative">
              <img :src="product.image" :alt="product.name" class="w-full h-56 object-cover">
              <span v-if="product.badge" class="absolute top-2 right-2 bg-red-500 text-white text-xs px-2 py-1 rounded-full">{{ product.badge }}</span>
            </div>
            <div class="p-4">
              <h3 class="text-lg font-semibold text-gray-800 mb-1">{{ product.name }}</h3>
              <div class="flex items-center mb-2">
                <div class="flex text-yellow-400">
                  <i v-for="i in 5" :key="i" :class="[ 'fas', i <= product.rating ? 'fa-star' : 'fa-star text-gray-300' ]"></i>
                </div>
                <span class="text-gray-500 text-sm ml-1">({{ product.reviewCount }})</span>
              </div>
              <div class="flex items-center justify-between">
                <div>
                  <span class="text-green-500 font-bold">${{ product.price.toFixed(2) }}</span>
                  <span v-if="product.originalPrice" class="text-gray-400 text-sm line-through ml-2">${{ product.originalPrice.toFixed(2) }}</span>
                </div>
                <button class="text-green-500 hover:text-green-600">
                  <i class="fas fa-shopping-cart"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="text-center mt-8">
          <a href="#" class="px-6 py-3 bg-green-500 text-white rounded-md hover:bg-green-600 transition-colors duration-200 inline-block">
            Shop All Products
          </a>
        </div>
      </div>
    </section>

    <!-- Testimonial Section -->
    <section class="py-12 bg-green-50">
      <div class="container mx-auto px-4">
        <h2 class="text-2xl md:text-3xl font-bold text-gray-800 text-center mb-8">What Our Customers Say</h2>
        <div class="flex flex-wrap justify-center gap-8">
          <div v-for="testimonial in testimonials" :key="testimonial.id" class="bg-white p-6 rounded-lg shadow-md w-full md:w-1/3 transform hover:scale-105 transition-transform duration-300">
            <p class="text-gray-600 mb-4">{{ testimonial.feedback }}</p>
            <div class="flex items-center">
              <img :src="testimonial.avatar" alt="Customer Avatar" class="w-12 h-12 rounded-full mr-4">
              <div>
                <p class="font-semibold text-gray-800">{{ testimonial.name }}</p>
                <p class="text-sm text-gray-500">{{ testimonial.position }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section class="py-12 bg-white">
      <div class="container mx-auto px-4">
        <h2 class="text-2xl md:text-3xl font-bold text-gray-800 text-center mb-8">Frequently Asked Questions</h2>
        <div class="space-y-6">
          <div v-for="faq in faqs" :key="faq.id" class="border-b border-gray-300 pb-4">
            <h3 class="text-xl font-semibold text-gray-800">{{ faq.question }}</h3>
            <p class="text-gray-600">{{ faq.answer }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Newsletter -->
    <section class="py-12 bg-green-500 text-white">
      <div class="container mx-auto px-4">
        <div class="max-w-2xl mx-auto text-center">
          <h2 class="text-2xl md:text-3xl font-bold mb-4">Join Our Wellness Community</h2>
          <p class="mb-8">Get exclusive health tips, product updates, and special offers directly to your inbox.</p>
          <div class="flex flex-col sm:flex-row gap-2 justify-center">
            <input type="email" placeholder="Your email address" class="px-4 py-3 rounded-md focus:outline-none text-gray-800 w-full sm:w-auto">
            <button class="px-6 py-3 bg-white text-green-500 font-medium rounded-md hover:bg-gray-100 transition-colors duration-200 w-full sm:w-auto">
              Subscribe
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-gray-300 py-12 mt-auto">
      <div class="container mx-auto px-4">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8 mb-8">
          <div>
            <div class="flex items-center space-x-2 mb-4">
              <i class="fas fa-leaf text-green-400 text-2xl"></i>
              <span class="text-2xl font-bold text-white">Vital<span class="text-green-400">Glam</span></span>
            </div>
            <p class="mb-4">Your trusted source for health information and premium wellness products.</p>
            <div class="flex space-x-4">
              <a href="#" class="text-gray-300 hover:text-white"><i class="fab fa-facebook-f"></i></a>
              <a href="#" class="text-gray-300 hover:text-white"><i class="fab fa-twitter"></i></a>
              <a href="#" class="text-gray-300 hover:text-white"><i class="fab fa-instagram"></i></a>
              <a href="#" class="text-gray-300 hover:text-white"><i class="fab fa-pinterest"></i></a>
            </div>
          </div>
          
          <div>
            <h3 class="text-lg font-semibold text-white mb-4">Quick Links</h3>
            <ul class="space-y-2">
              <li v-for="item in footerLinks.quickLinks" :key="item.id">
                <a :href="item.url" class="hover:text-white transition-colors duration-200">{{ item.title }}</a>
              </li>
            </ul>
          </div>
          
          <div>
            <h3 class="text-lg font-semibold text-white mb-4">Categories</h3>
            <ul class="space-y-2">
              <li v-for="item in footerLinks.categories" :key="item.id">
                <a :href="item.url" class="hover:text-white transition-colors duration-200">{{ item.title }}</a>
              </li>
            </ul>
          </div>
          
          <div>
            <h3 class="text-lg font-semibold text-white mb-4">Contact Us</h3>
            <ul class="space-y-2">
              <li class="flex items-start">
                <i class="fas fa-map-marker-alt mt-1 mr-2"></i>
                <span>123 Wellness Street, Healthy City, HC 12345</span>
              </li>
              <li class="flex items-center">
                <i class="fas fa-phone-alt mr-2"></i>
                <span>(123) 456-7890</span>
              </li>
              <li class="flex items-center">
                <i class="fas fa-envelope mr-2"></i>
                <span>support@vitalglam.com</span>
              </li>
            </ul>
          </div>
        </div>
        
        <div class="text-center text-gray-400">
          <p>&copy; 2025 VitalGlam. All Rights Reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      mobileMenuOpen: false,
      loadingPosts: false,
      loadingProducts: false,
      errorPosts: null,
      errorProducts: null,
      navItems: [
        { id: 1, title: "Home", url: "#" },
        { id: 2, title: "Blog", url: "#" },
        { id: 3, title: "Shop", url: "#" },
        { id: 4, title: "Contact", url: "#" }
      ],
      featuredPosts: [],
      featuredProducts: [],
      testimonials: [
        { id: 1, name: "John Doe", position: "Health Enthusiast", feedback: "VitalGlam has completely transformed my health journey. Highly recommend their products!", avatar: "/avatar1.jpg" },
        { id: 2, name: "Jane Smith", position: "Wellness Blogger", feedback: "The tips and products shared by VitalGlam have been so helpful. My energy levels are higher than ever.", avatar: "/avatar2.jpg" },
        { id: 3, name: "Emily Johnson", position: "Nutritionist", feedback: "As a nutritionist, I trust the products recommended by VitalGlam. They're truly top-notch!", avatar: "/avatar3.jpg" }
      ],
      faqs: [
        { id: 1, question: "How do I subscribe to your newsletter?", answer: "Simply enter your email address in the subscription box at the bottom of the page." },
        { id: 2, question: "Do you ship internationally?", answer: "Yes! We offer international shipping on all our products." },
        { id: 3, question: "Can I return a product?", answer: "Yes! We have a 30-day return policy on most products." }
      ],
      footerLinks: {
        quickLinks: [
          { id: 1, title: "Privacy Policy", url: "#" },
          { id: 2, title: "Terms & Conditions", url: "#" },
          { id: 3, title: "FAQ", url: "#" }
        ],
        categories: [
          { id: 1, title: "Health", url: "#" },
          { id: 2, title: "Nutrition", url: "#" },
          { id: 3, title: "Fitness", url: "#" },
          { id: 4, title: "Lifestyle", url: "#" }
        ]
      }
    };
  },
  methods: {
    toggleMobileMenu() {
      this.mobileMenuOpen = !this.mobileMenuOpen;
    },
    async fetchPosts() {
      this.loadingPosts = true;
      this.errorPosts = null;
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/posts/');
        this.featuredPosts = response.data;
      } catch (error) {
        this.errorPosts = 'Failed to load posts. Please try again later.';
        console.error('Error fetching posts:', error);
      } finally {
        this.loadingPosts = false;
      }
    },
    async fetchProducts() {
      this.loadingProducts = true;
      this.errorProducts = null;
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/products/');
        this.featuredProducts = response.data.map(product => ({
          ...product,
          price: parseFloat(product.price) || 0,
          originalPrice: parseFloat(product.originalPrice) || null,
          rating: Math.min(Math.max(product.rating || 0, 0), 5),
          reviewCount: product.reviewCount || 0,
          badge: product.badge || null,
        }));
      } catch (error) {
        this.errorProducts = 'Failed to load products. Please try again later.';
        console.error('Error fetching products:', error);
      } finally {
        this.loadingProducts = false;
      }
    },
  },
  created() {
    this.fetchPosts();
    this.fetchProducts();
  }
};
</script>

<style scoped>
/* Custom Animations */
@keyframes fade-in {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-fade-in {
  animation: fade-in 0.5s ease-out forwards;
}

.delay-100 {
  animation-delay: 0.1s;
}

.delay-200 {
  animation-delay: 0.2s;
}

.delay-300 {
  animation-delay: 0.3s;
}
</style>