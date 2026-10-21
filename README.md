# art-tech<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>青釉造物 · 耀州瓷 DIY 设计坊</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><circle cx='50' cy='50' r='40' fill='%232D5A4A'/><text x='50' y='62' font-size='36' text-anchor='middle' fill='%23F5F2EB' font-family='serif'>造</text></svg>">
<link rel="stylesheet" href="https://miaoda.feishu.cn/fonts/css2?family=Noto+Serif+SC:wght@400;600;700&family=Noto+Sans+SC:wght@300;400;500&display=swap">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  :root {
    --celadon-deep: #2D5A4A;
    --celadon-mid: #4A7D6B;
    --celadon-light: #7BA89A;
    --celadon-pale: #D4E4DE;
    --porcelain: #F5F2EB;
    --porcelain-white: #FDFBF7;
    --ochre: #8B5A2B;
    --ink: #3A3228;
    --ink-light: #6B6054;
  }
  html, body {
    height: 100%;
    font-family: 'Noto Sans SC', -apple-system, BlinkMacSystemFont, sans-serif;
    background: var(--porcelain);
    color: var(--ink);
    font-size: 15px;
    overflow-x: hidden;
  }
  body { display: flex; flex-direction: column; min-height: 100vh; }
  button { font-family: inherit; cursor: pointer; touch-action: manipulation; -webkit-tap-highlight-color: transparent; user-select: none; }

  /* ===== 顶部标题栏 ===== */
  .header {
    background: var(--celadon-deep);
    color: var(--porcelain);
    padding: 14px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-shrink: 0;
  }
  .header-title { font-family: 'Noto Serif SC', serif; font-size: 18px; font-weight: 600; letter-spacing: 2px; }
  .header-sub { font-size: 11px; opacity: 0.7; margin-top: 2px; letter-spacing: 1px; }
  .save-btn {
    background: rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.3);
    color: var(--porcelain);
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 13px;
    transition: background 0.2s;
  }
  .save-btn:hover { background: rgba(255,255,255,0.25); }
  .save-btn:active { transform: scale(0.95); }

  /* ===== 主区域 ===== */
  .main {
    flex: 1;
    display: flex;
    flex-direction: column;
    max-width: 900px;
    width: 100%;
    margin: 0 auto;
    padding: 16px;
    gap: 16px;
  }

  /* ===== 预览画布区 ===== */
  .canvas-area {
    background: var(--porcelain-white);
    border-radius: 16px;
    box-shadow: 0 2px 12px rgba(45, 90, 74, 0.1);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    min-height: 320px;
    position: relative;
    overflow: hidden;
  }
  .canvas-area::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image: radial-gradient(circle at 1px 1px, rgba(45,90,74,0.05) 1px, transparent 0);
    background-size: 20px 20px;
    pointer-events: none;
  }
  #canvas {
    width: 100%;
    max-width: 520px;
    aspect-ratio: 1 / 1;
    height: auto;
    display: block;
    position: relative;
    z-index: 1;
    border-radius: 8px;
  }

  /* ===== 控制面板 ===== */
  .control-panel {
    background: var(--porcelain-white);
    border-radius: 16px;
    box-shadow: 0 2px 12px rgba(45, 90, 74, 0.1);
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .tabs { display: flex; border-bottom: 1px solid var(--celadon-pale); }
  .tab {
    flex: 1;
    padding: 14px;
    text-align: center;
    font-size: 14px;
    color: var(--ink-light);
    border: none;
    background: transparent;
    border-bottom: 2px solid transparent;
    transition: all 0.2s;
  }
  .tab.active { color: var(--celadon-deep); border-bottom-color: var(--celadon-deep); font-weight: 500; }
  .tab:active { background: var(--celadon-pale); }
  .panel-body { flex: 1; display: flex; flex-direction: column; min-width: 0; }
  .tab-content { padding: 16px; display: none; max-height: 300px; overflow-y: auto; }
  .tab-content.active { display: block; }

  /* ===== 选项网格 ===== */
  .option-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
  .option-item {
    aspect-ratio: 1;
    border: 2px solid var(--celadon-pale);
    border-radius: 12px;
    background: var(--porcelain);
    cursor: pointer;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 4px;
    transition: all 0.15s;
    padding: 6px;
    touch-action: manipulation;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }
  .option-item:hover { border-color: var(--celadon-light); }
  .option-item.selected { border-color: var(--celadon-deep); background: var(--celadon-pale); }
  .option-item:active { transform: scale(0.94); }
  .option-icon { width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; }
  .option-label { font-size: 11px; color: var(--ink); text-align: center; }

  /* ===== 釉色色块 ===== */
  .color-swatches { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
  .swatch {
    aspect-ratio: 1;
    border-radius: 12px;
    cursor: pointer;
    border: 3px solid transparent;
    transition: all 0.15s;
    position: relative;
    touch-action: manipulation;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }
  .swatch.selected { border-color: var(--celadon-deep); transform: scale(1.05); }
  .swatch:active { transform: scale(0.94); }
  .swatch-label {
    position: absolute;
    bottom: -20px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 10px;
    color: var(--ink-light);
    white-space: nowrap;
  }
  .color-group { margin-bottom: 24px; }
  .color-group-title { font-size: 13px; color: var(--ink-light); margin-bottom: 12px; font-weight: 500; }

  /* ===== 当前作品信息 ===== */
  .work-info {
    background: var(--porcelain-white);
    border-radius: 12px;
    padding: 14px 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
  }
  .work-name { font-family: 'Noto Serif SC', serif; font-size: 15px; color: var(--celadon-deep); font-weight: 600; }
  .work-desc { color: var(--ink-light); font-size: 12px; margin-top: 2px; }
  .work-icon { font-size: 26px; opacity: 0.28; font-family: 'Noto Serif SC', serif; }

  /* ===== 随机生成按钮 ===== */
  .random-wrap { padding: 0 16px 16px; }
  .random-btn {
    width: 100%;
    padding: 12px;
    background: linear-gradient(135deg, var(--celadon-deep), var(--celadon-mid));
    color: var(--porcelain);
    border: none;
    border-radius: 12px;
    font-size: 14px;
    transition: transform 0.15s;
  }
  .random-btn:active { transform: scale(0.98); }

  /* ===== 桌面端布局 ===== */
  @media (min-width: 768px) {
    .main { flex-direction: row; padding: 24px; gap: 20px; }
    .canvas-area { flex: 1.5; min-height: 500px; }
    .control-panel { flex: 1; max-width: 340px; flex-direction: row; }
    .tabs { flex-direction: column; border-bottom: none; border-right: 1px solid var(--celadon-pale); }
    .tab {
      border-bottom: none;
      border-right: 2px solid transparent;
      padding: 16px 12px;
      text-align: left;
      padding-left: 20px;
    }
    .tab.active { border-right-color: var(--celadon-deep); border-bottom-color: transparent; }
    .tab-content { max-height: 420px; }
  }
</style>
</head>
<body>

<header class="header">
  <div>
    <div class="header-title">青釉造物</div>
    <div class="header-sub">耀州瓷 DIY 设计坊</div>
  </div>
  <button class="save-btn" id="saveBtn">保存作品</button>
</header>

<div class="main">
  <!-- 预览区 -->
  <div class="canvas-area">
    <canvas id="canvas"></canvas>
  </div>

  <!-- 控制面板 -->
  <div class="control-panel">
    <div class="tabs">
      <button class="tab active" data-tab="shape">器型</button>
      <button class="tab" data-tab="pattern">纹样</button>
      <button class="tab" data-tab="glaze">釉色</button>
    </div>
    <div class="panel-body">
      <div class="tab-content active" id="tab-shape">
        <div class="option-grid" id="grid-shape"></div>
      </div>
      <div class="tab-content" id="tab-pattern">
        <div class="option-grid" id="grid-pattern"></div>
      </div>
      <div class="tab-content" id="tab-glaze">
        <div class="color-group">
          <div class="color-group-title">青釉系列</div>
          <div class="color-swatches" id="grid-glaze-celadon"></div>
        </div>
        <div class="color-group">
          <div class="color-group-title">其他釉色</div>
          <div class="color-swatches" id="grid-glaze-other"></div>
        </div>
      </div>
      <div class="random-wrap">
        <button class="random-btn" id="randomBtn">随机生成一件耀州瓷</button>
      </div>
    </div>
  </div>

  <!-- 当前作品信息 -->
  <div class="work-info">
    <div>
      <div class="work-name" id="workName"></div>
      <div class="work-desc" id="workDesc"></div>
    </div>
    <div class="work-icon" id="workIcon"></div>
  </div>
</div>

<script>
'use strict';

/* ===================== 工具 ===================== */
function rand(n) { return Math.floor(Math.random() * n); }
function mulberry32(a) {
  return function () {
    a |= 0; a = (a + 0x6D2B79F5) | 0;
    let t = Math.imul(a ^ (a >>> 15), 1 | a);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}

/* ===================== 数据 ===================== */
const GLAZES = [
  { id: 'olive', label: '橄榄青', name: '橄榄青釉', group: 'celadon', main: '#5E7F6C', light: '#93B3A2', dark: '#33584A', hi: '#EAF3EE', edge: '#24473B', crackle: true },
  { id: 'pale',  label: '淡青釉', name: '淡青釉',   group: 'celadon', main: '#93B9A9', light: '#C2D8CC', dark: '#6FA08E', hi: '#F0F7F3', edge: '#4E7A69', crackle: true },
  { id: 'deep',  label: '深青釉', name: '深青釉',   group: 'celadon', main: '#3B6253', light: '#5F8675', dark: '#24402F', hi: '#DCEBE2', edge: '#16291F', crackle: true },
  { id: 'ice',   label: '天青釉', name: '天青釉',   group: 'celadon', main: '#A9CCC0', light: '#D3E6DE', dark: '#7FB0A0', hi: '#F5FAF7', edge: '#5B8A79', crackle: true },
  { id: 'bean',  label: '豆青釉', name: '豆青釉',   group: 'celadon', main: '#7FA487', light: '#A9C4AE', dark: '#5B8266', hi: '#EEF5EF', edge: '#406047', crackle: true },
  { id: 'pink',  label: '粉青釉', name: '粉青釉',   group: 'celadon', main: '#A9C3B3', light: '#CBDCD1', dark: '#85A393', hi: '#F2F6F3', edge: '#5F7A6B', crackle: true },
  { id: 'white', label: '月白釉', name: '月白釉',   group: 'other',   main: '#F0EBDF', light: '#FBF8F0', dark: '#D6CDB8', hi: '#FFFFFF', edge: '#B5A988', crackle: false },
  { id: 'cream', label: '米黄釉', name: '米黄釉',   group: 'other',   main: '#E8DCC4', light: '#F5EDDA', dark: '#C9B794', hi: '#FFFDF7', edge: '#A78F63', crackle: false },
  { id: 'tea',   label: '茶叶末', name: '茶叶末釉', group: 'other',   main: '#8A7A58', light: '#A99B79', dark: '#67593C', hi: '#E9E2D0', edge: '#4A3F2A', crackle: false },
  { id: 'black', label: '黑釉',   name: '黑釉',     group: 'other',   main: '#3E3833', light: '#5C544C', dark: '#231F1B', hi: '#8A8178', edge: '#120F0D', crackle: false }
];
const GLAZE_MAP = Object.fromEntries(GLAZES.map(g => [g.id, g]));

const SHAPES = [
  {
    id: 'bowl', label: '碗', iconChar: '碗',
    icon: '<ellipse cx="20" cy="14" rx="14" ry="4"/><path d="M6,14 Q6,30 20,32 Q34,30 34,14"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 74, cy - 26);
      body.quadraticCurveTo(cx - 76, cy + 52, cx, cy + 62);
      body.quadraticCurveTo(cx + 76, cy + 52, cx + 74, cy - 26);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      opening(cx, cy - 26, 74, 13, g);
      footring(cx, cy + 60, 27, 6, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy - 24, 50, g);
    }
  },
  {
    id: 'plate', label: '盘', iconChar: '盘',
    icon: '<ellipse cx="20" cy="18" rx="16" ry="5"/><path d="M4,18 Q6,26 20,28 Q34,26 36,18"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.ellipse(cx, cy, 92, 27, 0, 0, Math.PI * 2);
      fillGlaze(body, cx, cy, g);
      // 盘心（内凹）
      ctx.fillStyle = g.dark;
      ctx.beginPath(); ctx.ellipse(cx, cy - 2, 66, 17, 0, 0, Math.PI * 2); ctx.fill();
      ctx.fillStyle = g.light; ctx.globalAlpha = 0.35;
      ctx.beginPath(); ctx.ellipse(cx, cy - 4, 58, 14, 0, 0, Math.PI * 2); ctx.fill();
      ctx.globalAlpha = 1;
      ctx.strokeStyle = g.edge; ctx.lineWidth = 1.6; ctx.globalAlpha = 0.5;
      ctx.beginPath(); ctx.ellipse(cx, cy, 92, 27, 0, 0, Math.PI * 2); ctx.stroke();
      ctx.globalAlpha = 1;
      footring(cx, cy + 24, 34, 6, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy - 3, 48, g);
    }
  },
  {
    id: 'vase', label: '玉壶春瓶', iconChar: '瓶',
    icon: '<path d="M16,6 L24,6 L23,12 Q30,16 28,26 Q26,34 20,34 Q14,34 12,26 Q10,16 17,12 Z"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 9, cy - 88); body.lineTo(cx + 9, cy - 88);
      body.lineTo(cx + 11, cy - 58);
      body.quadraticCurveTo(cx + 24, cy - 38, cx + 48, cy - 2);
      body.quadraticCurveTo(cx + 58, cy + 30, cx + 40, cy + 58);
      body.quadraticCurveTo(cx + 30, cy + 72, cx, cy + 72);
      body.quadraticCurveTo(cx - 30, cy + 72, cx - 40, cy + 58);
      body.quadraticCurveTo(cx - 58, cy + 30, cx - 48, cy - 2);
      body.quadraticCurveTo(cx - 24, cy - 38, cx - 11, cy - 58);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      opening(cx, cy - 88, 9, 3, g);
      footring(cx, cy + 70, 22, 5, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy + 6, 34, g);
    }
  },
  {
    id: 'jar', label: '罐', iconChar: '罐',
    icon: '<ellipse cx="20" cy="8" rx="6" ry="2"/><path d="M14,8 L14,12 Q8,16 8,24 Q8,32 20,32 Q32,32 32,24 Q32,16 26,12 L26,8"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 22, cy - 58); body.lineTo(cx + 22, cy - 58); body.lineTo(cx + 20, cy - 42);
      body.quadraticCurveTo(cx + 55, cy - 18, cx + 55, cy + 20);
      body.quadraticCurveTo(cx + 55, cy + 62, cx, cy + 64);
      body.quadraticCurveTo(cx - 55, cy + 62, cx - 55, cy + 20);
      body.quadraticCurveTo(cx - 55, cy - 18, cx - 20, cy - 42);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      opening(cx, cy - 58, 22, 6, g);
      footring(cx, cy + 62, 24, 5, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy + 8, 40, g);
    }
  },
  {
    id: 'meiping', label: '梅瓶', iconChar: '梅',
    icon: '<path d="M17,5 L23,5 L22,10 Q28,14 29,22 Q30,32 20,34 Q10,32 11,22 Q12,14 18,10 Z"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 8, cy - 92); body.lineTo(cx + 8, cy - 92);
      body.lineTo(cx + 7, cy - 78);
      body.quadraticCurveTo(cx + 40, cy - 52, cx + 46, cy - 6);
      body.quadraticCurveTo(cx + 48, cy + 40, cx + 30, cy + 66);
      body.quadraticCurveTo(cx + 24, cy + 72, cx, cy + 72);
      body.quadraticCurveTo(cx - 24, cy + 72, cx - 30, cy + 66);
      body.quadraticCurveTo(cx - 48, cy + 40, cx - 46, cy - 6);
      body.quadraticCurveTo(cx - 40, cy - 52, cx - 7, cy - 78);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      opening(cx, cy - 92, 8, 3, g);
      footring(cx, cy + 70, 16, 4, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy + 12, 26, g);
    }
  },
  {
    id: 'ewer', label: '执壶', iconChar: '壶',
    icon: '<path d="M14,8 L22,8 L22,12 Q28,16 26,28 Q24,34 18,34 Q12,34 12,28 Q10,16 16,12 Z"/><path d="M26,14 Q32,16 30,22"/>',
    draw(cx, cy, g, st) {
      // 1. 把手（底层闭合形体，画在壶身后，避免穿模）
      const handle = new Path2D();
      handle.moveTo(cx - 38, cy - 32);
      handle.quadraticCurveTo(cx - 56, cy - 10, cx - 54, cy + 10);
      handle.quadraticCurveTo(cx - 52, cy + 34, cx - 38, cy + 36);
      handle.quadraticCurveTo(cx - 46, cy + 12, cx - 44, cy - 8);
      handle.quadraticCurveTo(cx - 45, cy - 24, cx - 38, cy - 32);
      handle.closePath();
      plainFill(handle, cx, cy, g);
      // 2. 壶身
      const body = new Path2D();
      body.moveTo(cx - 12, cy - 70); body.lineTo(cx + 12, cy - 70); body.lineTo(cx + 10, cy - 50);
      body.quadraticCurveTo(cx + 40, cy - 30, cx + 38, cy + 20);
      body.quadraticCurveTo(cx + 36, cy + 65, cx, cy + 65);
      body.quadraticCurveTo(cx - 36, cy + 65, cx - 38, cy + 20);
      body.quadraticCurveTo(cx - 40, cy - 30, cx - 10, cy - 50);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      // 3. 壶嘴（与壶身平滑接合）
      const spout = new Path2D();
      spout.moveTo(cx + 38, cy - 42);
      spout.quadraticCurveTo(cx + 54, cy - 32, cx + 53, cy - 14);
      spout.quadraticCurveTo(cx + 50, cy - 6, cx + 38, cy - 4);
      spout.quadraticCurveTo(cx + 40, cy - 22, cx + 38, cy - 42);
      spout.closePath();
      plainFill(spout, cx, cy, g);
      opening(cx, cy - 70, 12, 3.5, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy + 5, 34, g);
    }
  },
  {
    id: 'pillow', label: '瓷枕', iconChar: '枕',
    icon: '<rect x="6" y="14" width="28" height="14" rx="4"/><line x1="10" y1="28" x2="10" y2="32"/><line x1="30" y1="28" x2="30" y2="32"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 72, cy - 8);
      body.quadraticCurveTo(cx - 72, cy - 26, cx, cy - 26);
      body.quadraticCurveTo(cx + 72, cy - 26, cx + 72, cy - 8);
      body.lineTo(cx + 72, cy + 18);
      body.quadraticCurveTo(cx + 72, cy + 30, cx, cy + 30);
      body.quadraticCurveTo(cx - 72, cy + 30, cx - 72, cy + 18);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      ctx.fillStyle = g.dark;
      ctx.fillRect(cx - 56, cy + 26, 12, 12);
      ctx.fillRect(cx + 44, cy + 26, 12, 12);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy - 2, 52, g);
    }
  },
  {
    id: 'washer', label: '洗', iconChar: '洗',
    icon: '<ellipse cx="20" cy="16" rx="14" ry="5"/><path d="M6,16 Q6,26 20,28 Q34,26 34,16"/>',
    draw(cx, cy, g, st) {
      const body = new Path2D();
      body.moveTo(cx - 62, cy - 6);
      body.quadraticCurveTo(cx - 62, cy + 32, cx, cy + 36);
      body.quadraticCurveTo(cx + 62, cy + 32, cx + 62, cy - 6);
      body.closePath();
      fillGlaze(body, cx, cy, g);
      opening(cx, cy - 6, 62, 11, g);
      footring(cx, cy + 34, 24, 5, g);
      if (st.pattern !== 'plain') carvePattern(st.pattern, cx, cy - 6, 42, g);
    }
  }
];
const SHAPE_MAP = Object.fromEntries(SHAPES.map(s => [s.id, s]));

const PATTERNS = [
  { id: 'plain', label: '素面', decor: '素面', icon: '<circle cx="20" cy="20" r="12"/>', build: () => [] },
  {
    id: 'lotus', label: '莲瓣纹', decor: '刻花莲瓣纹',
    icon: '<path d="M20,10 Q24,16 20,22 Q16,16 20,10 Z"/><ellipse cx="20" cy="28" rx="8" ry="3"/>',
    build(cx, cy, r) {
      const paths = [];
      for (let i = 0; i < 8; i++) {
        const a = (i / 8) * Math.PI * 2;
        const px = cx + Math.cos(a) * r * 0.55, py = cy + Math.sin(a) * r * 0.45;
        const p = new Path2D();
        p.ellipse(px, py, r * 0.22, r * 0.34, a, 0, Math.PI * 2);
        paths.push(p);
      }
      const c = new Path2D(); c.ellipse(cx, cy, r * 0.16, r * 0.22, 0, 0, Math.PI * 2); paths.push(c);
      return paths;
    }
  },
  {
    id: 'peony', label: '缠枝牡丹', decor: '刻花缠枝牡丹',
    icon: '<circle cx="20" cy="18" r="4"/><circle cx="12" cy="18" r="4"/><circle cx="28" cy="18" r="4"/><circle cx="20" cy="26" r="4"/>',
    build(cx, cy, r) {
      const paths = [];
      const c = new Path2D(); c.arc(cx, cy, r * 0.22, 0, Math.PI * 2); paths.push(c);
      for (let i = 0; i < 7; i++) {
        const a = (i / 7) * Math.PI * 2;
        const p = new Path2D();
        p.ellipse(cx + Math.cos(a) * r * 0.34, cy + Math.sin(a) * r * 0.3, r * 0.2, r * 0.12, a, 0, Math.PI * 2);
        paths.push(p);
      }
      for (const dir of [-1, 1]) {
        const s = new Path2D();
        s.moveTo(cx - dir * r * 0.2, cy + r * 0.4);
        s.bezierCurveTo(cx - dir * r * 0.7, cy + r * 0.15, cx - dir * r * 0.7, cy - r * 0.15, cx - dir * r * 0.2, cy - r * 0.4);
        paths.push(s);
        const leaf = new Path2D();
        leaf.ellipse(cx - dir * r * 0.55, cy + r * 0.02, r * 0.14, r * 0.07, dir * 0.6, 0, Math.PI * 2);
        paths.push(leaf);
      }
      return paths;
    }
  },
  {
    id: 'wave', label: '海浪纹', decor: '刻花海浪纹',
    icon: '<path d="M6,20 Q10,14 14,20 Q18,26 22,20 Q26,14 30,20 Q34,26 38,20"/>',
    build(cx, cy, r) {
      const paths = [];
      for (let row = 0; row < 3; row++) {
        const y = cy - r * 0.35 + row * r * 0.35;
        const p = new Path2D();
        p.moveTo(cx - r * 0.7, y);
        for (let x = -r * 0.7; x <= r * 0.7; x += 6) {
          p.lineTo(cx + x, y + Math.sin(x * 0.09 + row) * 4);
        }
        paths.push(p);
      }
      return paths;
    }
  },
  {
    id: 'kids', label: '婴戏纹', decor: '刻花婴戏纹',
    icon: '<circle cx="14" cy="14" r="3"/><circle cx="26" cy="14" r="3"/><path d="M14,17 L14,26 M26,17 L26,26 M10,22 L18,22 M22,22 L30,22"/>',
    build(cx, cy, r) {
      const paths = [];
      for (const side of [-1, 1]) {
        const hx = cx + side * r * 0.35, hy = cy - r * 0.15;
        const head = new Path2D(); head.arc(hx, hy - r * 0.3, r * 0.14, 0, Math.PI * 2); paths.push(head);
        const body = new Path2D(); body.moveTo(hx, hy - r * 0.16); body.lineTo(hx, hy + r * 0.25); paths.push(body);
        const arm = new Path2D(); arm.moveTo(hx - side * r * 0.14, hy); arm.lineTo(hx + side * r * 0.14, hy); paths.push(arm);
        const leg1 = new Path2D(); leg1.moveTo(hx, hy + r * 0.25); leg1.lineTo(hx - side * r * 0.08, hy + r * 0.4); paths.push(leg1);
        const leg2 = new Path2D(); leg2.moveTo(hx, hy + r * 0.25); leg2.lineTo(hx + side * r * 0.08, hy + r * 0.4); paths.push(leg2);
      }
      const ball = new Path2D(); ball.arc(cx, cy + r * 0.35, r * 0.1, 0, Math.PI * 2); paths.push(ball);
      return paths;
    }
  },
  {
    id: 'chrys', label: '菊纹', decor: '刻花菊纹',
    icon: '<circle cx="20" cy="20" r="3"/><path d="M20,10 L20,17 M20,23 L20,30 M10,20 L17,20 M23,20 L30,20 M13,13 L18,18 M22,22 L27,27 M27,13 L22,18 M18,22 L13,27"/>',
    build(cx, cy, r) {
      const paths = [];
      const c = new Path2D(); c.arc(cx, cy, r * 0.1, 0, Math.PI * 2); paths.push(c);
      for (let i = 0; i < 12; i++) {
        const a = (i / 12) * Math.PI * 2;
        const p = new Path2D();
        p.moveTo(cx + Math.cos(a) * r * 0.14, cy + Math.sin(a) * r * 0.14);
        p.lineTo(cx + Math.cos(a) * r * 0.5, cy + Math.sin(a) * r * 0.5);
        paths.push(p);
      }
      const o = new Path2D(); o.arc(cx, cy, r * 0.32, 0, Math.PI * 2); paths.push(o);
      return paths;
    }
  },
  {
    id: 'meander', label: '回纹', decor: '刻花回纹',
    icon: '<path d="M8,12 L16,12 L16,18 L12,18 L12,15 L20,15 L20,22 L28,22 L28,18 L24,18"/><path d="M8,28 L16,28 L16,24 L12,24" opacity="0.5"/>',
    build(cx, cy, r) {
      const paths = [];
      const u = r * 0.5, n = 4;
      const startX = cx - (n * u) / 2, y = cy;
      for (let i = 0; i < n; i++) {
        const x = startX + i * u;
        const p = new Path2D();
        p.moveTo(x, y - u);
        p.lineTo(x + u * 0.8, y - u);
        p.lineTo(x + u * 0.8, y - u * 0.4);
        p.lineTo(x + u * 0.4, y - u * 0.4);
        p.lineTo(x + u * 0.4, y);
        p.lineTo(x + u * 0.8, y);
        paths.push(p);
      }
      return paths;
    }
  },
  {
    id: 'grass', label: '卷草纹', decor: '刻花卷草纹',
    icon: '<path d="M8,20 Q14,12 20,20 Q26,28 32,20"/><path d="M12,26 Q16,22 20,26 Q24,30 28,26" opacity="0.5"/>',
    build(cx, cy, r) {
      const paths = [];
      const p = new Path2D();
      p.moveTo(cx - r * 0.6, cy);
      for (let t = 0; t <= 1; t += 0.02) {
        const x = cx - r * 0.6 + t * r * 1.2;
        const y = cy + Math.sin(t * Math.PI * 3) * r * 0.28;
        p.lineTo(x, y);
      }
      paths.push(p);
      for (let i = 0; i < 5; i++) {
        const t = 0.1 + i * 0.2;
        const x = cx - r * 0.6 + t * r * 1.2;
        const y = cy + Math.sin(t * Math.PI * 3) * r * 0.28;
        const leaf = new Path2D();
        leaf.ellipse(x, y - r * 0.12, r * 0.1, r * 0.16, 0.5, 0, Math.PI * 2);
        paths.push(leaf);
      }
      return paths;
    }
  }
];
const PATTERN_MAP = Object.fromEntries(PATTERNS.map(p => [p.id, p]));

/* ===================== 开片纹理（预生成，确定性） ===================== */
const crackle = document.createElement('canvas');
crackle.width = crackle.height = 500;
(function makeCrackle() {
  const cc = crackle.getContext('2d');
  const rng = mulberry32(20261021);
  cc.strokeStyle = '#000';
  cc.lineWidth = 0.7;
  for (let i = 0; i < 34; i++) {
    let x = rng() * 500, y = rng() * 500;
    let ang = rng() * Math.PI * 2;
    cc.beginPath();
    cc.moveTo(x, y);
    const segs = 3 + Math.floor(rng() * 6);
    for (let s = 0; s < segs; s++) {
      ang += (rng() - 0.5) * 1.3;
      const len = 14 + rng() * 30;
      x += Math.cos(ang) * len; y += Math.sin(ang) * len;
      cc.lineTo(x, y);
    }
    cc.stroke();
  }
})();

/* ===================== 画布与渲染 ===================== */
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

const state = { shape: 'bowl', pattern: 'plain', glaze: 'olive' };
let fadeRAF = 0;
let shuffleTimer = 0;

function drawShadow(cx, cy) {
  ctx.fillStyle = 'rgba(80,70,55,0.10)';
  ctx.beginPath(); ctx.ellipse(cx, cy + 135, 120, 26, 0, 0, Math.PI * 2); ctx.fill();
  ctx.fillStyle = 'rgba(80,70,55,0.14)';
  ctx.beginPath(); ctx.ellipse(cx, cy + 125, 70, 12, 0, 0, Math.PI * 2); ctx.fill();
}

/* 釉面填充：基础渐变 → 裁剪 → 高光 → 边缘加深 → 开片 → 外轮廓 */
function fillGlaze(path, cx, cy, g) {
  const grad = ctx.createLinearGradient(cx - 100, cy - 110, cx + 100, cy + 110);
  grad.addColorStop(0, g.light);
  grad.addColorStop(0.45, g.main);
  grad.addColorStop(1, g.dark);
  ctx.fillStyle = grad;
  ctx.fill(path);

  ctx.save();
  ctx.clip(path);
  // 镜面高光（左上柔光）
  const hi = ctx.createRadialGradient(cx - 50, cy - 80, 4, cx - 50, cy - 80, 150);
  hi.addColorStop(0, g.hi);
  hi.addColorStop(1, 'rgba(255,255,255,0)');
  ctx.fillStyle = hi;
  ctx.globalAlpha = 0.5;
  ctx.fillRect(cx - 180, cy - 180, 360, 360);
  ctx.globalAlpha = 1;
  // 边缘加深（内阴影，给体积感）
  ctx.strokeStyle = g.dark;
  ctx.lineWidth = 9;
  ctx.globalAlpha = 0.22;
  ctx.stroke(path);
  ctx.globalAlpha = 1;
  // 开片
  if (g.crackle) {
    ctx.globalAlpha = 0.13;
    ctx.drawImage(crackle, cx - 250, cy - 250, 500, 500);
    ctx.globalAlpha = 1;
  }
  ctx.restore();
  // 锐利外轮廓
  ctx.strokeStyle = g.edge;
  ctx.lineWidth = 1.4;
  ctx.globalAlpha = 0.5;
  ctx.stroke(path);
  ctx.globalAlpha = 1;
}

/* 简单釉面填充（把手、壶嘴等附属件） */
function plainFill(path, cx, cy, g) {
  const grad = ctx.createLinearGradient(cx - 80, cy - 60, cx + 80, cy + 70);
  grad.addColorStop(0, g.light); grad.addColorStop(0.5, g.main); grad.addColorStop(1, g.dark);
  ctx.fillStyle = grad;
  ctx.fill(path);
  ctx.strokeStyle = g.edge; ctx.lineWidth = 1.2; ctx.globalAlpha = 0.45;
  ctx.stroke(path);
  ctx.globalAlpha = 1;
}

/* 口沿（内部开口，径向渐变出深度 + 左前高光） */
function opening(cx, cy, rx, ry, g) {
  const grad = ctx.createRadialGradient(cx, cy - ry * 0.2, 0, cx, cy, rx);
  grad.addColorStop(0, g.dark);
  grad.addColorStop(1, g.edge);
  ctx.fillStyle = grad;
  ctx.beginPath(); ctx.ellipse(cx, cy, rx, ry, 0, 0, Math.PI * 2); ctx.fill();
  ctx.strokeStyle = g.light; ctx.lineWidth = 1.4; ctx.globalAlpha = 0.55;
  ctx.beginPath(); ctx.ellipse(cx, cy, rx, ry, -0.4, Math.PI * 0.6, Math.PI * 1.7); ctx.stroke();
  ctx.globalAlpha = 1;
}

/* 底足 */
function footring(cx, cy, rx, ry, g) {
  ctx.fillStyle = g.dark;
  ctx.beginPath(); ctx.ellipse(cx, cy, rx, ry, 0, 0, Math.PI * 2); ctx.fill();
  ctx.fillStyle = g.edge;
  ctx.beginPath(); ctx.ellipse(cx, cy, rx * 0.7, ry * 0.7, 0, 0, Math.PI * 2); ctx.fill();
}

/* 刻花纹样：深色凹槽 + 浅色浮雕边双描 */
function carvePattern(type, cx, cy, r, g) {
  const p = PATTERN_MAP[type];
  if (!p || type === 'plain') return;
  const paths = p.build(cx, cy, r);
  for (const path of paths) {
    ctx.save();
    ctx.strokeStyle = g.light; ctx.lineWidth = 1.8; ctx.globalAlpha = 0.55;
    ctx.translate(0.9, -0.9); ctx.stroke(path);
    ctx.restore();
    ctx.strokeStyle = g.dark; ctx.lineWidth = 1.2; ctx.globalAlpha = 0.85;
    ctx.stroke(path);
  }
  ctx.globalAlpha = 1;
}

/* 在 500×500 逻辑坐标内绘制一整套瓷器 */
function drawScene(st) {
  const g = GLAZE_MAP[st.glaze];
  const cx = 250, cy = 250;
  drawShadow(cx, cy);
  SHAPE_MAP[st.shape].draw(cx, cy, g, st);
}

/* 渲染到主画布（处理 DPR、响应式缩放、背景、展示台柔光） */
function render(st) {
  const dpr = window.devicePixelRatio || 1;
  const cssW = canvas.clientWidth || 500;
  const cssH = canvas.clientHeight || 500;
  canvas.width = Math.round(cssW * dpr);
  canvas.height = Math.round(cssH * dpr);
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

  ctx.fillStyle = '#FBF8F2';
  ctx.fillRect(0, 0, cssW, cssH);
  const glow = ctx.createRadialGradient(cssW / 2, cssH / 2, 10, cssW / 2, cssH / 2, Math.max(cssW, cssH) * 0.6);
  glow.addColorStop(0, 'rgba(90,120,105,0.06)');
  glow.addColorStop(1, 'rgba(90,120,105,0)');
  ctx.fillStyle = glow;
  ctx.fillRect(0, 0, cssW, cssH);

  const s = Math.min(cssW, cssH) / 500;
  ctx.save();
  ctx.translate((cssW - 500 * s) / 2, (cssH - 500 * s) / 2);
  ctx.scale(s, s);
  drawScene(st);
  ctx.restore();

  updateWorkInfo(st);
}

function updateWorkInfo(st) {
  const g = GLAZE_MAP[st.glaze];
  const sh = SHAPE_MAP[st.shape];
  const pt = PATTERN_MAP[st.pattern];
  document.getElementById('workName').textContent = `${g.name} ${sh.label}`;
  document.getElementById('workDesc').textContent = `${pt.decor} · 宋代风格`;
  document.getElementById('workIcon').textContent = sh.iconChar;
}

/* ===================== 构建选项面板 ===================== */
function optionEl(group, id, icon, label) {
  const d = document.createElement('div');
  d.className = 'option-item';
  d.dataset[group] = id;
  d.innerHTML = `<div class="option-icon"><svg viewBox="0 0 40 40" fill="none" stroke="currentColor" stroke-width="1.5">${icon}</svg></div><div class="option-label">${label}</div>`;
  d.addEventListener('click', () => select(group, id));
  return d;
}
function swatchEl(g) {
  const d = document.createElement('div');
  d.className = 'swatch';
  d.dataset.glaze = g.id;
  d.style.background = g.main;
  d.innerHTML = `<div class="swatch-label">${g.label}</div>`;
  d.addEventListener('click', () => select('glaze', g.id));
  return d;
}
function buildOptions() {
  const shapeGrid = document.getElementById('grid-shape');
  const patternGrid = document.getElementById('grid-pattern');
  const celadonGrid = document.getElementById('grid-glaze-celadon');
  const otherGrid = document.getElementById('grid-glaze-other');
  SHAPES.forEach(s => shapeGrid.appendChild(optionEl('shape', s.id, s.icon, s.label)));
  PATTERNS.forEach(p => patternGrid.appendChild(optionEl('pattern', p.id, p.icon, p.label)));
  GLAZES.filter(g => g.group === 'celadon').forEach(g => celadonGrid.appendChild(swatchEl(g)));
  GLAZES.filter(g => g.group === 'other').forEach(g => otherGrid.appendChild(swatchEl(g)));
}

/* ===================== 交互 ===================== */
function switchTab(name) {
  document.querySelectorAll('.tab').forEach(t => t.classList.toggle('active', t.dataset.tab === name));
  document.querySelectorAll('.tab-content').forEach(t => t.classList.toggle('active', t.id === 'tab-' + name));
}
document.querySelectorAll('.tab').forEach(t => t.addEventListener('click', () => switchTab(t.dataset.tab)));

function syncSelection() {
  document.querySelectorAll('[data-shape]').forEach(el => el.classList.toggle('selected', el.dataset.shape === state.shape));
  document.querySelectorAll('[data-pattern]').forEach(el => el.classList.toggle('selected', el.dataset.pattern === state.pattern));
  document.querySelectorAll('[data-glaze]').forEach(el => el.classList.toggle('selected', el.dataset.glaze === state.glaze));
}

function select(group, id) {
  const prev = { shape: state.shape, pattern: state.pattern, glaze: state.glaze };
  state[group] = id;
  syncSelection();
  crossfade(prev, state);
}

/* 切换时的 200ms 交叉淡入淡出 */
function crossfade(from, to) {
  const prev = document.createElement('canvas');
  prev.width = canvas.width; prev.height = canvas.height;
  prev.getContext('2d').drawImage(canvas, 0, 0);

  render(to);
  cancelAnimationFrame(fadeRAF);
  const t0 = performance.now(), D = 200;
  function step(t) {
    const a = 1 - (t - t0) / D;
    if (a <= 0) return;
    render(to);
    ctx.save();
    ctx.setTransform(1, 0, 0, 1, 0, 0);
    ctx.globalAlpha = Math.max(0, a);
    ctx.drawImage(prev, 0, 0);
    ctx.restore();
    fadeRAF = requestAnimationFrame(step);
  }
  fadeRAF = requestAnimationFrame(step);
}

function randomPick() {
  const ids = {
    shape: SHAPES.map(s => s.id),
    pattern: PATTERNS.map(p => p.id),
    glaze: GLAZES.map(g => g.id)
  };
  let st, tries = 0;
  do {
    st = {
      shape: ids.shape[rand(ids.shape.length)],
      pattern: ids.pattern[rand(ids.pattern.length)],
      glaze: ids.glaze[rand(ids.glaze.length)]
    };
    tries++;
  } while (tries < 10 && st.shape === state.shape && st.pattern === state.pattern && st.glaze === state.glaze);
  return st;
}

function randomDesign() {
  const target = randomPick();
  let i = 0, N = 4;
  cancelAnimationFrame(fadeRAF);
  clearInterval(shuffleTimer);
  shuffleTimer = setInterval(() => {
    i++;
    if (i >= N) {
      clearInterval(shuffleTimer);
      const prev = { shape: state.shape, pattern: state.pattern, glaze: state.glaze };
      state.shape = target.shape; state.pattern = target.pattern; state.glaze = target.glaze;
      syncSelection();
      crossfade(prev, state);
      return;
    }
    state.shape = SHAPES[rand(SHAPES.length)].id;
    state.pattern = PATTERNS[rand(PATTERNS.length)].id;
    state.glaze = GLAZES[rand(GLAZES.length)].id;
    render(state);
  }, 65);
}

function saveWork() {
  const link = document.createElement('a');
  link.download = '耀州瓷DIY作品.png';
  link.href = canvas.toDataURL('image/png');
  link.click();
}

document.getElementById('saveBtn').addEventListener('click', saveWork);
document.getElementById('randomBtn').addEventListener('click', randomDesign);

/* ===================== 初始化 ===================== */
window.addEventListener('resize', () => { cancelAnimationFrame(fadeRAF); render(state); });
buildOptions();
syncSelection();
render(state);
</script>
</body>
</html>
