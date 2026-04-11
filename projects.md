---
title: Projects
layout: default
permalink: /projects/
---

<section class="min-h-screen py-16 bg-zinc-950">
  <div class="max-w-6xl mx-auto px-6">
    <div class="text-center mb-16">
      <h1 class="text-5xl font-bold tracking-tighter text-white">Projects</h1>
      <p class="text-zinc-400 mt-3 text-xl">Security engineering tools, research, and implementations</p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      {% for project in site.projects %}
      {% assign repo_slug = project.github | replace: 'https://github.com/', '' %}
      
      <a href="{{ project.github }}" target="_blank" class="group block">
        <div class="bg-zinc-900 border border-zinc-700 rounded-3xl overflow-hidden shadow-2xl transition-all duration-300 hover:-translate-y-3 hover:border-zinc-400 hover:shadow-zinc-400/30">
          
          <!-- Minimal window title bar (clean, no lights, no text) -->
          <div class="h-11 bg-zinc-800 border-b border-zinc-700"></div>

          <!-- Image container -->
          <div class="relative h-72">
            <img src="{{ project.image }}" 
                 alt="{{ project.title }}"
                 class="absolute inset-0 w-full h-full object-cover transition-transform duration-500 group-hover:scale-110">

            <!-- Stronger dark gradient for perfect text readability -->
            <div class="absolute inset-0 bg-gradient-to-t from-zinc-950/95 via-zinc-950/60 to-transparent"></div>

            <!-- GitHub stars badge (top right) -->
            <div class="absolute top-6 right-6">
              <img src="https://img.shields.io/github/stars/{{ repo_slug }}?style=flat-square&amp;color=64748b&amp;logo=github&amp;logoColor=white" 
                   alt="GitHub stars" 
                   class="h-5 drop-shadow-md">
            </div>

            <!-- Title + Description area -->
            <div class="absolute bottom-8 left-8 right-8 transition-all duration-300">
              <!-- Default: Title -->
              <h3 class="text-3xl font-semibold text-white tracking-tight transition-all duration-300 group-hover:opacity-0">
                {{ project.title }}
              </h3>
              
              <!-- Hover: Short description replaces title -->
              <p class="text-zinc-300 text-base leading-tight mt-1 opacity-0 transition-all duration-300 group-hover:opacity-100 line-clamp-3">
                {{ project.description }}
              </p>
            </div>
          </div>
        </div>
      </a>
      {% endfor %}
    </div>
  </div>
</section>