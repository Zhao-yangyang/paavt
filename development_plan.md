# 成绩分析与可视化工具 - 开发步骤

**阶段一：核心结构与数据输入 (基础搭建) - 已完成**

1.  **步骤 1.1：增强 HTML 结构 - 添加输入元素和占位符。 - 已完成**
    *   **`upload-section` (上传区域):** 添加一个 `<input type="file" id="csvFile" accept=".csv">` 文件选择框。添加一个小段落 `<p>` 来提示用户预期的 CSV 文件格式 (依据 PRD FR-01.4)。
    *   **`preview-section` (预览区域):** 添加一个 `<div>`，例如 `id="dataPreviewTableContainer"`，用于显示 CSV 数据的预览表格。
    *   **`settings-section` (设置区域):** 为"及格线"添加输入框 (例如 `<input type="number" id="passScore" value="60">`)，为"优秀线"添加输入框 (例如 `<input type="number" id="excellentScore" value="85">`)。
    *   **`results-section` (结果区域):** 为不同的结果表格添加 `div` 占位符 (例如 `subjectStatsTableContainer`, `studentRankingsTableContainer`, `scoreDistributionTableContainer`)。添加一个"导出分析结果"按钮 (`<button id="exportResultsBtn">`)。
    *   **`visualization-section` (可视化区域):**
        *   添加一个 `<select id="chartTypeSelect">` 下拉菜单，包含选项如"分数分布直方图"、"成绩等级饼图"。
        *   添加一个 `<canvas id="analysisChart">` 元素，Chart.js 将在这里渲染图表。
        *   添加一个"导出图表"按钮 (`<button id="exportChartBtn">`)。
2.  **步骤 1.2：基础 CSS 样式。 - 已完成**
    *   在 `<style>` 标签内添加更具体的 CSS 规则，以改善输入框、按钮、表格（添加后）的外观，并确保布局整洁。(依据 PRD NFR-02, NFR-04)

**阶段二：JavaScript - 数据处理与基础分析**

3.  **步骤 2.1：实现 CSV 解析与预览 (依据 PRD FR-01.1, FR-01.2, FR-01.3)。 - 已完成**
    *   在 `<script>` 标签内：
        *   为 `csvFile` 输入框添加事件监听器。
        *   当文件被选择时，使用 `PapaParse` 解析文件。
        *   处理成功解析的情况：
            *   存储解析后的数据（对象数组）。
            *   动态生成 HTML 表格，在 `dataPreviewTableContainer` 中显示数据的前几行（例如 5-10 行）。
            *   通过更改 `display` 样式，取消隐藏 `preview-section`, `settings-section`, `results-section` 和 `visualization-section`。
        *   处理解析错误，并向用户显示警告或消息。
4.  **步骤 2.2：实现核心分析函数 (依据 PRD FR-02.1, FR-02.2, FR-02.3 - 初步计算)。 - 已完成**
    *   创建 JavaScript 函数来实现：
        *   识别科目列（目前假设除了"姓名"或"学号"之外的所有列都是科目）。
        *   计算每个学生的总分和平均分。
        *   对每个科目：计算平均分、最高分、最低分、中位数。
        *   （暂时占位）根据默认或用户输入的阈值计算及格/优秀人数和比率。
        *   根据总分计算学生排名。
        *   （暂时占位）计算分数段分布。
    *   这些计算结果应存储在合适的 JavaScript 数据结构中。
5.  **步骤 2.3：显示基础分析结果 (依据 PRD FR-02.1, FR-02.2)。 - 已完成**
    *   在 `subjectStatsTableContainer`（显示每科的平均分、最高分、最低分、中位数）和 `studentRankingsTableContainer`（显示学生姓名、各科成绩、总分、平均分、排名）中动态填充 HTML 表格。

**阶段三：JavaScript - 高级分析、可视化与导出 - 已完成**

6.  **步骤 3.1：实现设置交互 (依据 PRD FR-05.3) 并更新分析。 - 已完成**
    *   为 `passScore` 和 `excellentScore` 输入框添加事件监听器。
    *   当这些值改变时，重新触及格/优秀率和分数段分布的计算。更新显示结果的相关部分。
7.  **步骤 3.2：实现数据可视化 (依据 PRD FR-03.1, FR-03.2)。 - 已完成**
    *   根据 `chartTypeSelect` 中的选择：
        *   **分数分布直方图**: 为所选科目或总分准备数据（统计各分数区间的学生人数）。使用 `Chart.js` 在 `analysisChart` 画布上渲染条形图。
        *   **成绩等级饼图**: 为所选科目或总分准备数据（统计优秀、良好、及格、不及格等各等级的学生人数）。使用 `Chart.js` 渲染饼图。
    *   确保在数据或设置更改时图表能自动更新。
8.  **步骤 3.3：实现结果导出 (依据 PRD FR-04.1)。 - 已完成**
    *   为 `exportResultsBtn` 添加功能，将分析数据（例如，学生排名、科目统计）编译成 CSV 字符串并触发下载。
9.  **步骤 3.4：实现图表导出 (依据 PRD FR-04.2)。 - 已完成**
    *   为 `exportChartBtn` 添加功能，使用 `Chart.js` 的 `toBase64Image()` 方法，并触发下载当前显示的图表为 PNG 图片。

**阶段四：优化与测试**

10. **步骤 4.1：测试与调试 (依据 PRD NFR-01, NFR-03)。**
    *   使用各种有效和无效的 CSV 文件进行全面测试。
    *   在不同的现代浏览器上测试所有功能。
    *   调试发现的任何问题。
11. **步骤 4.2：UI/UX 优化 (依据 PRD NFR-02)。 - 进行中**
    *   为耗时较长的操作（如果有）添加加载指示器。
    *   改进错误提示和用户反馈。
    *   确保界面直观易用。
    *   已根据用户要求，更新UI样式，使其更现代化、类似macOS风格，并适合中小学生审美。
12. **步骤 4.3：代码审查与清理 (依据 PRD NFR-04)。**
    *   审查 JavaScript 代码的清晰性、效率和注释。
    *   确保 CSS 组织良好。 