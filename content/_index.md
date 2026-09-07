---
title: "主页"
description: "主页展示的内容。"
---

<!-- 👇👇👇 以下内容将显示在 Hero 图片下方 👇👇👇 -->

<div class="prose prose-invert max-w-prose mx-auto px-6 py-12">  

<!-- 核心修改：父容器添加 text-center -->
<div class="prose prose-invert max-w-prose mx-auto px-6 py-12 text-center">
<!-- 标题：保持居中 -->
<h2 class="text-2xl md:text-3xl font-bold mb-6 text-white tracking-wide">
    得墨个人工作室
</h2>
<!-- 正文：移除 text-left，继承父级居中 -->
<p class="text-lg text-slate-300 leading-relaxed">
    拒绝平庸，坚持设计驱动。<br>
    构建 <span class="text-cyan-400 font-medium">物理世界</span> 与 <span class="text-cyan-400 font-medium">数字世界</span> 的桥梁。
</p>
</div>

<!-- 父容器：控制两列布局 -->
<div class="grid grid-cols-1 md:grid-cols-2 gap-8 my-10">
<!-- 模块 1 -->
<div class="mt-8 bg-slate-800/50 p-6 rounded-xl border border-slate-700 hover:border-cyan-500 transition-colors">
    <h3 class="text-xl font-bold text-cyan-400 mb-3">💻 软件主导</h3>
        <p class="text-slate-400">
            从底层驱动到上层应用，我们提供全栈嵌入式解决方案。擅长 RTOS 移植、Linux 应用开发及复杂的算法实现。
        </p>
</div>

<!-- 模块 2 -->
<div class="mt-8 bg-slate-800/50 p-6 rounded-xl border border-slate-700 hover:border-orange-500 transition-colors">
    <h3 class="text-xl font-bold text-orange-400 mb-3">⚡ 硬件辅助</h3>
      <p class="text-slate-400">
        精通原理图设计与 PCB Layout。我们理解硬件的局限性，从而写出更高效的代码，实现软硬结合的最佳性能。
      </p>
</div>

 <div class="mt-8 bg-slate-800/50 p-6 rounded-xl border border-slate-700 hover:border-orange-500 transition-colors">
        <h3 class="text-xl font-bold text-white mb-2">💡 核心要义</h3>
        <p class="text-lg text-slate-300 leading-relaxed">
            拒绝千篇一律的公版方案。<strong class="text-orange-400">哪怕只有一片板子</strong>，亦倾注心血，提供从硬件到软件的全方位深度定制！
        </p>
    </div>

</div>

  <h3 class="mt-12 mb-6 text-center">技术栈</h3>
<div class="flex flex-wrap justify-center gap-4">
    <span class="px-4 py-2 bg-slate-800 rounded-full text-sm font-mono text-cyan-300 border border-slate-700">STM32 / ESP32</span>
    <span class="px-4 py-2 bg-slate-800 rounded-full text-sm font-mono text-cyan-300 border border-slate-700">FreeRTOS / Linux</span>
    <span class="px-4 py-2 bg-slate-800 rounded-full text-sm font-mono text-cyan-300 border border-slate-700">BLE</span>
    <span class="px-4 py-2 bg-slate-800 rounded-full text-sm font-mono text-cyan-300 border border-slate-700">MQTT / IoT</span>
    <span class="px-4 py-2 bg-slate-800 rounded-full text-sm font-mono text-cyan-300 border border-slate-700">RV-C</span>
  </div>
</div>


<!-- {{< list title = "精选案例" limit = 3 cardView = true where = "Section" value = "cases">}} -->

