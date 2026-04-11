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
        <div class="bg-zinc-900 border border-zinc-700 rounded-3xl overflow-hidden shadow-2xl transition-all duration-300 hover:-translate-y-3 hover:border-emerald-500/60 hover:shadow-emerald-500/20">
          
          <!-- Window title bar -->
          <div class="h-11 bg-zinc-800 flex items-center px-4 border-b border-zinc-700">
            <div class="flex gap-1.5">
              <div class="w-3 h-3 rounded-full bg-red-500"></div>
              <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
              <div class="w-3 h-3 rounded-full bg-emerald-500"></div>
            </div>
            <div class="flex-1 text-center">
              <span class="text-zinc-400 text-xs font-mono tracking-widest">project</span>
            </div>
          </div>

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