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
      <a href="{{ project.github }}" target="_blank" class="group block">
        <div class="bg-zinc-900 border border-zinc-700 rounded-3xl overflow-hidden shadow-2xl transition-all duration-300 hover:-translate-y-3 hover:border-zinc-400 hover:shadow-zinc-400/30">
          
          <!-- Minimal window title bar (no lights, no text) -->
          <div class="h-11 bg-zinc-800 border-b border-zinc-700"></div>

          <!-- Image + title overlay -->
          <div class="relative h-72">
            <img src="{{ project.image }}" 
                 alt="{{ project.title }}"
                 class="absolute inset-0 w-full h-full object-cover transition-transform duration-500 group-hover:scale-110">
            <div class="absolute inset-0 bg-gradient-to-t from-zinc-950/90 via-zinc-950/30 to-transparent"></div>
            
            <div class="absolute bottom-8 left-8 right-8">
              <h3 class="text-3xl font-semibold text-white tracking-tight">{{ project.title }}</h3>
            </div>
          </div>
        </div>
      </a>
      {% endfor %}
    </div>
  </div>
</section>