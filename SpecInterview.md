1. 專案概述
本專案目標為開發一個基於 Vue 3 的高度可自定義 Knob (旋鈕) 元件。該元件需具備直觀的滑鼠/觸控互動功能，並能透過外圈環形進度條視覺化顯示當前數值。
2. 開發限制與環境 (Constraints)
* 核心框架： 必須使用 Vue 3。
* 語言選擇： JavaScript 
* 依賴限制： 嚴禁使用任何第三方 UI 框架或功能套件（僅能使用 Vue 內建 API）。
* 樣式工具： 允許使用 Tailwind CSS
* 架構要求： 必須封裝為獨立的 可複用元件 (Component)。
3. 功能需求 (Functional Requirements)
3.1 互動邏輯
* 拖拽變更： 使用者透過滑鼠點擊並拖拽元件時，需即時更新數值。
* 事件通知： 元件需設計 Emit Events (如 update:modelValue)，確保父組件能接收到最新的數值變化。
3.2 視覺規格
* 核心顯示： 元件中心需以數字顯示當前數值。
* 環形進度： 外圈需有一個環形條，隨數值大小同步增減長度。
* 外觀定義： 支援開發者透過 Props 自定義 顏色 (Color) 與 尺寸 (Size)。
3.3 數值配置
* 範疇控制： 需提供 Props 設定 最小值 (Min) 與 最大值 (Max)。
* 起始角度： 支援設定進度條的 起始點位置（例如：由 12 點鐘方向或 6 點鐘方向開始）。

4. 技術實作建議 (Technical Design)
4.1 座標與角度運算
實作旋鈕的核心在於將滑鼠的螢幕座標轉換為圓周角度。
通常建議使用 Math.atan2(y, x) 來計算，公式如下：
$$\theta = \arctan2(y_{mouse} - y_{center}, x_{mouse} - x_{center})$$
4.2 SVG 進度條實現
建議使用 SVG 的 <circle> 標籤，並透過控制 stroke-dasharray 與 stroke-dashoffset 來實現環形進度的增長效果。
4.3 介面設計 (API Props)
Prop Name	Type	Description
modelValue	Number	當前數值 (v-model)
min	Number	最小值設定
max	Number	最大值設定
size	Number/String	元件直徑大小
primaryColor	String	進度條與重點顏色
startAngle	Number	起始旋轉角度
5. 驗收標準 (Acceptance Criteria)
1. 流暢度： 拖拽數值時不應有明顯延遲。
2. 精準度： 數值應嚴格限制在 Min/Max 範圍內。
3. 相容性： 元件在不同尺寸下應保持視覺比例正常。
4. 程式碼品質： 邏輯清晰，Props 與 Events 命名符合 Vue 慣例。
