---
title: Projects
layout: default
permalink: /projects/
---

<div class="max-w-6xl mx-auto px-6 py-10">
  <!-- Header now matches Home + Info pages exactly -->
  <header class="mb-12">
    <h1 class="text-4xl font-bold tracking-tight">Projects</h1>
    <p class="text-zinc-400 mt-3 text-xl">Security engineering tools, research, and implementations</p>
  </header>

  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
    {% for project in site.projects %}
    <a href="{{ project.github }}" target="_blank" class="group block">
      <div class="bg-zinc-900 border border-zinc-700 rounded-3xl overflow-hidden shadow-2xl transition-all duration-300 hover:-translate-y-3 hover:border-zinc-400 hover:shadow-zinc-400/30">
        
        <!-- Minimal clean window title bar -->
        <div class="h-11 bg-zinc-800 border-b border-zinc-700"></div>

        <!-- Image container -->
        <div class="relative h-72">
          <img src="{{ project.image }}" 
               alt="{{ project.title }}"
               class="absolute inset-0 w-full h-full object-cover transition-transform duration-500 group-hover:scale-110">

          <!-- Strong dark gradient for perfect text readability -->
          <div class="absolute inset-0 bg-gradient-to-t from-zinc-950/95 via-zinc-950/70 to-transparent"></div>

          <!-- Title + Description (title always visible) -->
          <div class="absolute bottom-8 left-8 right-8 flex flex-col transition-all duration-300 group-hover:-translate-y-4">
            <h3 class="text-3xl font-semibold text-white tracking-tight">{{ project.title }}</h3>
            
            <!-- Description appears underneath on hover -->
            <p class="text-zinc-300 text-base leading-tight mt-3 opacity-0 max-h-0 overflow-hidden transition-all duration-300 group-hover:opacity-100 group-hover:max-h-28">
              {{ project.description }}
            </p>
          </div>
        </div>
      </div>
    </a>
    {% endfor %}
  </div>
</div>