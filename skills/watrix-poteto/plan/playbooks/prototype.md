### Prototype
<!-- 中文：原型验证 -->

**You own the design decision, not the code. The prototype is a throwaway instrument. The real build follows Feature.**
<!-- 中文：你负责的是设计决策，不是生产代码。 -->
<!-- 中文：原型是用完即可丢弃的验证工具。 -->
<!-- 中文：实际实现遵循 Feature 流程。 -->

The one playbook where the Laziness Protocol's "smallest change" and the verification bar invert. Speed over polish, code quality does not matter, no planning. The rigor is in picking the right design cheaply. Propose variations the user didn't ask for, throw an approach away and try another.
<!-- 中文：这是唯一一个把 Laziness Protocol 中的最小改动要求和验收标准反过来使用的 playbook。 -->
<!-- 中文：速度优先于打磨，代码质量不重要，也不做详细计划。 -->
<!-- 中文：严谨之处在于用低成本选中合适的设计。 -->
<!-- 中文：可以提出用户未提到的变体；一条路不合适，就丢掉它再试另一条。 -->

1. Scope the decision the prototype exists to make: which layout, which interaction, which density, or for an empirical fork which behavior, timing, or approach. No decision means no prototype. Route to Feature.
   <!-- 中文：先明确原型要帮助决定什么：布局、交互、信息密度，或需要实测的行为、时序和实现路径。 -->
   <!-- 中文：没有待定的问题就不做原型。 -->
   <!-- 中文：转入 Feature 流程。 -->
2. Gather references when the design space is open. Search for prior art, summarize a moodboard of themes, palettes, and layouts, let the user pick directions before building. Skip when the direction is set.
   <!-- 中文：设计方向还开放时先收集参考。 -->
   <!-- 中文：找已有做法，把主题、配色和布局整理成参考板，由用户先选方向再实现。 -->
   <!-- 中文：已有明确方向，就跳过。 -->
3. Build throwaway in an isolated scratch dir, separate from production source. For a visual decision, vanilla HTML/CSS/JS or the lightest stack that renders the idea, CDN deps, a dev server with hot reload. For a behavioral or timing decision, the smallest script that exercises the question. No production framework, no tests, no abstractions.
   <!-- 中文：在与生产源码隔离的临时目录做可丢弃原型。 -->
   <!-- 中文：视觉决策用原生 HTML、CSS、JS，或能呈现想法的最轻工具栈；依赖可走 CDN，开发服务支持热重载。 -->
   <!-- 中文：行为或时序决策用能触发目标现象的最小脚本。 -->
   <!-- 中文：不用生产框架，不写测试，不做抽象。 -->
4. When comparing alternatives, build them behind one switcher (buttons or a keypress), each variant labeled. This is the **exhaust-the-design-space** principle skill made cheap.
   <!-- 中文：比较方案时放在同一个切换器里，用按钮或按键切换，并标明每个变体。 -->
   <!-- 中文：这就是低成本应用 exhaust-the-design-space 原则。 -->
5. Verify on the matching surface. For a visual decision, screenshot each variant via the control skill and drive the interaction. For a behavioral or timing decision, observe the thing you are deciding by logging the timing, printing the output, or watching the render. The observation is the test here, not an assertion.
   <!-- 中文：在相应的实际界面或环境验证。 -->
   <!-- 中文：视觉决策通过操控技能给每个变体截图，再实际操作交互。 -->
   <!-- 中文：行为或时序决策通过记时、打印输出或观察渲染，直接查看要作判断的现象。 -->
   <!-- 中文：这里用观察验证，不写断言。 -->
6. Present alternatives, tradeoffs, and a recommendation. The output is the decision plus the throwaway artifact, not shippable code. Hand the chosen direction to **Feature** (or `architect` for the shape) for the real build.
   <!-- 中文：给出备选方案、取舍和推荐。 -->
   <!-- 中文：交付物是决策和可丢弃原型，不是可发布的代码。 -->
   <!-- 中文：把选定方向交给 Feature 做正式实现；若需要先定结构，则交给 architect。 -->

**Reply:** the variants explored, the evidence (screenshots for a visual decision, the observed output or timing for a behavioral one), tradeoffs, your recommendation, and the scratch path. Say plainly that the prototype is throwaway.
<!-- 中文：回复：探索过的变体、证据（视觉决策用截图，行为决策用观察到的输出或耗时）、取舍、建议和临时目录路径。 -->
<!-- 中文：明确说明原型用完即可丢弃。 -->
