---
layout: page
search_exclude: true
---

<div class="min-h-screen font-sans bg-gray-700">

  <!-- Jumbotron -->
  <section 
    class="text-center py-20 text-white bg-cover bg-center relative"
    style="background-image: url('{{ site.baseurl }}/images/biomed.webp');"
  >
    <div class="absolute inset-0 bg-gradient-to-r from-green-900 via-green-800 to-green-700 opacity-90"></div>
    <div class="relative z-10">
      <h1 class="text-5xl md:text-6xl font-extrabold mb-4">
        Welcome to <span class="text-green-300">Bio-SKANZ</span>
      </h1>
      <p class="text-lg text-gray-200 max-w-2xl mx-auto">
        Engaging and informative games designed to help you learn about Scripps Research biomedical contributions and advancements.
      </p>
    </div>
  </section>

  <!-- Cards Section -->
  <section class="py-16 bg-gray-700 text-white">
    <div class="max-w-6xl mx-auto px-4 grid md:grid-cols-3 gap-8">
      <div class="bg-gray-800 rounded-2xl shadow-lg p-6 flex flex-col justify-between transition transform hover:scale-105 hover:shadow-2xl hover:ring-2 hover:ring-green-400">
        <div>
          <h3 class="text-2xl font-semibold mb-2 text-green-300">Lab Simulation</h3>
          <p class="text-gray-300">Try our hands-on lab simulation and simulate building DNA.</p>
        </div>
        <a href="#" class="mt-4 inline-block px-4 py-2 bg-green-500 text-white rounded-xl hover:bg-green-300 hover:text-gray-900 shadow-md transition">
        Explore
        </a>
      </div>
      <div class="bg-gray-800 rounded-2xl shadow-lg p-6 flex flex-col justify-between transition transform hover:scale-105 hover:shadow-2xl hover:ring-2 hover:ring-green-400">
        <div>
          <h3 class="text-2xl font-semibold mb-2 text-green-300">Trivia Game</h3>
          <p class="text-gray-300">Test your biomedical knowledge with our difficulty-adaptive game.</p>
        </div>
        <a href="{{ site.baseurl }}/labsim2.0" class="mt-4 inline-block px-4 py-2 bg-green-500 text-white rounded-xl hover:bg-green-300 hover:text-gray-900 shadow-md transition">
        Explore
        </a>
      </div>
      <div class="bg-gray-800 rounded-2xl shadow-lg p-6 flex flex-col justify-between transition transform hover:scale-105 hover:shadow-2xl hover:ring-2 hover:ring-green-400">
        <div>
          <h3 class="text-2xl font-semibold mb-2 text-green-300">Question Predictor</h3>
          <p class="text-gray-300">Ask questions and get real-time feedback on related topics.</p>
        </div>
        <a href="{{ site.baseurl }}/sciencequestions" class="mt-4 inline-block px-4 py-2 bg-green-500 text-white rounded-xl hover:bg-green-300 hover:text-gray-900 shadow-md transition">
        Explore
        </a>
      </div>
    </div>
  </section>

</div>
