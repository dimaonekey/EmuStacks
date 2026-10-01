<div align="center">

<h1>EMUSTACKS</h1>
<p><i>High-Performance Hybrid Android Runtime &amp; mimgui Client</i></p>

<p>
  <img src="https://img.shields.io/badge/C++17-2b2b30?style=for-the-badge&logo=cplusplus&logoColor=c0c0c8" alt="C++17" />
  <img src="https://img.shields.io/badge/DirectX_11-36363d?style=for-the-badge&logo=windows&logoColor=c0c0c8" alt="DirectX 11" />
  <img src="https://img.shields.io/badge/Lua_5.4-2b2b30?style=for-the-badge&logo=lua&logoColor=c0c0c8" alt="Lua 5.4" />
  <img src="https://img.shields.io/badge/Dear_ImGui-36363d?style=for-the-badge&logo=imgui&logoColor=c0c0c8" alt="ImGui" />
  <img src="https://img.shields.io/badge/QEMU_WHPX-2b2b30?style=for-the-badge&logo=qemu&logoColor=c0c0c8" alt="QEMU" />
  <img src="https://img.shields.io/badge/Linux_uinput-36363d?style=for-the-badge&logo=linux&logoColor=c0c0c8" alt="uinput" />
</p>

<p>
  <a href="#english">English</a> • <a href="#русский">Русский</a>
</p>

</div>

<hr />

<div id="english">

<h2>🌐 English</h2>

<h3>Overview</h3>
<p>
  <b>EmuStacks</b> is an optimized, lightweight Android virtualization shell for Windows. It hijacks existing emulator viewports (BlueStacks / LDPlayer) or boots standalone QEMU instances into a unified, dark-slate <b>Dear ImGui (mimgui)</b> frame featuring hot-reloadable Lua scripting and kernel-level touch injection.
</p>

<h3>Architecture Workflow</h3>
<pre><code>  [ User Input: Keyboard WASD / Mouse Delta ]
                     │
                     ▼
 ┌────────────────────────────────────────────────────────┐
 │ EmuStacks Host (DirectX 11 / ImGui / Lua 5.4)         │
 │  • Docked Android Viewport (Win32 Hijack / QEMU)       │
 │  • UI Layer rendered live via scripts/interface.lua    │
 │  • Transforms keystrokes to polar touch coordinates    │
 └───────────────────────┬────────────────────────────────┘
                         │
                         ▼  TCP Stream (:8888, TCP_NODELAY)
 ┌────────────────────────────────────────────────────────┐
 │ In-Guest Daemon (emustacks_daemon)                    │
 │  • Real-time non-blocking epoll packet receiver        │
 │  • Injects touch events directly via /dev/uinput       │
 └───────────────────────┬────────────────────────────────┘
                         │
                         ▼
          [ Android Framework / Game Engine ]</code></pre>

<h3>Key Features</h3>
<ul>
  <li><b>Window Hijacking:</b> Strips telemetry bars, vendor ads, and borders from BlueStacks/LDPlayer, seamlessly docking the render surface into the custom frame.</li>
  <li><b>Pure Lua Interface:</b> UI logic, fonts (Tahoma &amp; Trebuchet MS), and widgets reside in <code>scripts/interface.lua</code> for runtime hot-reloading.</li>
  <li><b>Kernel Touch Injection:</b> Bypasses high-latency ADB pipes with an in-guest C daemon routing events straight into <code>/dev/uinput</code>.</li>
  <li><b>RawInput FPS Lock:</b> Press <code>F10</code> to capture hardware mouse deltas and map them directly to camera look sweeps.</li>
</ul>

<h3>Control Matrix</h3>
<table width="100%">
  <thead>
    <tr>
      <th align="left">Input</th>
      <th align="left">Target Action</th>
      <th align="left">Mapping Mode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>W A S D</code></td>
      <td>Virtual Directional Pad</td>
      <td>Multi-Touch Polar Vector (Slot 0)</td>
    </tr>
    <tr>
      <td><code>Mouse Move</code></td>
      <td>Camera / Aim Look</td>
      <td>Touch Sweep Interpolation (Slot 9)</td>
    </tr>
    <tr>
      <td><code>F10</code></td>
      <td>FPS Aim Toggle</td>
      <td>RawInput Mouse Clip &amp; Lock</td>
    </tr>
    <tr>
      <td><code>Space</code></td>
      <td>Primary Action / Jump</td>
      <td>Direct Virtual Touch (Slot 1)</td>
    </tr>
    <tr>
      <td><code>Left Shift</code></td>
      <td>Sprint / Boost</td>
      <td>Direct Virtual Touch (Slot 7)</td>
    </tr>
    <tr>
      <td><code>Esc</code> / UI Buttons</td>
      <td>Android Navigation</td>
      <td>Hardware Keycodes (158 / 102 / 580)</td>
    </tr>
  </tbody>
</table>

</div>

<hr />

<div id="русский">

<h2>🇷🇺 Русский</h2>

<h3>О проекте</h3>
<p>
  <b>EmuStacks</b> — высокопроизводительная среда виртуализации Android для Windows. Проект перехватывает окна сторонних эмуляторов (BlueStacks / LDPlayer) или запускает изолированный QEMU внутри единой графитовой оболочки <b>Dear ImGui (в стиле mimgui)</b> с поддержкой горячей перезагрузки Lua-интерфейса и прямого ввода через ядро Linux.
</p>

<h3>Схема работы</h3>
<pre><code>  [ Ввод пользователя: Клавиатура WASD / Смещение мыши ]
                     │
                     ▼
 ┌────────────────────────────────────────────────────────┐
 │ EmuStacks Host (DirectX 11 / ImGui / Lua 5.4)         │
 │  • Захваченное окно Android (Win32 Hijack / QEMU)      │
 │  • Интерфейс рендерится из scripts/interface.lua       │
 │  • Преобразование клавиш в векторные координаты тача   │
 └───────────────────────┬────────────────────────────────┘
                         │
                         ▼  TCP-сокет (:8888, без задержек)
 ┌────────────────────────────────────────────────────────┐
 │ Гостевой демон (emustacks_daemon)                     │
 │  • Неблокирующий сокет с обработкой через epoll        │
 │  • Прямой инжект нажатий в ядро через /dev/uinput      │
 └───────────────────────┬────────────────────────────────┘
                         │
                         ▼
        [ Подсистема Android / Мобильная игра ]</code></pre>

<h3>Возможности</h3>
<ul>
  <li><b>Захват окна (Window Hijack):</b> Встраивание рабочего окна BlueStacks или LDPlayer с полным вырезанием рекламы, внешних рамок и лишних панелей.</li>
  <li><b>Интерфейс на чистом Lua:</b> Все панели, шрифты (Tahoma и Trebuchet MS) и кнопки настраиваются в файле <code>scripts/interface.lua</code> и обновляются без перезапуска.</li>
  <li><b>Нулевая задержка ввода:</b> Собственный C-демон внутри Android генерирует аппаратные события тачскрина через <code>/dev/uinput</code> в обход медленного ADB.</li>
  <li><b>FPS-режим (захват мыши):</b> Нажатие <code>F10</code> блокирует курсор в границах окна и преобразует движения мыши в плавный поворот камеры.</li>
</ul>

<h3>Управление</h3>
<table width="100%">
  <thead>
    <tr>
      <th align="left">Клавиша</th>
      <th align="left">Действие</th>
      <th align="left">Тип обработки</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>W A S D</code></td>
      <td>Движение персонажа</td>
      <td>Векторный мультитач джойстик (Slot 0)</td>
    </tr>
    <tr>
      <td><code>Движение мыши</code></td>
      <td>Обзор / Прицел</td>
      <td>Свайп сенсорной зоны (Slot 9)</td>
    </tr>
    <tr>
      <td><code>F10</code></td>
      <td>Шутерный режим</td>
      <td>Аппаратная фиксация курсора (RawInput)</td>
    </tr>
    <tr>
      <td><code>Space</code></td>
      <td>Прыжок / Действие</td>
      <td>Виртуальное касание (Slot 1)</td>
    </tr>
    <tr>
      <td><code>Left Shift</code></td>
      <td>Спринт</td>
      <td>Виртуальное касание (Slot 7)</td>
    </tr>
    <tr>
      <td><code>Esc</code> / Кнопки GUI</td>
      <td>Назад / Домой / Задачи</td>
      <td>Аппаратные скан-коды (158 / 102 / 580)</td>
    </tr>
  </tbody>
</table>

</div>

<hr />

<div align="center">
  <sub>EmuStacks Runtime Subsystem • Distributed under the MIT License • 2026</sub>
</div>
