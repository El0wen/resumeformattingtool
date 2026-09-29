# resumeformattingtool
简历排版自动化程序
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>简历工坊 · Resume Studio</title>
<style>
/* ================= 基础 ================= */
*{box-sizing:border-box}
html,body{margin:0;height:100%}
body{
  font-family:-apple-system,BlinkMacSystemFont,"PingFang SC","Microsoft YaHei",sans-serif;
  background:#eef1f6;color:#1f2937;overflow:hidden;
}
button{font-family:inherit}
.modal{position:fixed;inset:0;background:rgba(15,23,42,.5);z-index:100;display:none;align-items:center;justify-content:center;padding:20px;backdrop-filter:blur(3px)}
.modal.show{display:flex}
.modal-box{background:#fff;border-radius:14px;max-width:820px;width:100%;max-height:88vh;display:flex;flex-direction:column;box-shadow:0 20px 60px rgba(15,23,42,.32);overflow:hidden}
.modal-head{display:flex;align-items:center;justify-content:space-between;padding:14px 18px;border-bottom:1px solid #e8eef5}
.modal-head h2{margin:0;font-size:16px;color:#0f172a;display:flex;align-items:center;gap:8px}
.modal-x{border:none;background:transparent;font-size:22px;color:#94a3b8;cursor:pointer;line-height:1;padding:0 4px}
.modal-x:hover{color:#e11d48}
.modal-body{padding:16px 18px;overflow-y:auto;font-size:13.5px;line-height:1.75;color:#334155}
.modal-body h3{margin:16px 0 8px;font-size:14px;color:#1B3A5C}
.modal-body h3:first-child{margin-top:0}
.modal-body ol,.modal-body ul{margin:6px 0;padding-left:20px}
.modal-body li{margin-bottom:4px}
.modal-body code{background:#f1f5f9;color:#1B3A5C;padding:1px 5px;border-radius:4px;font-size:12px}

/* ============================================================
 *  新手引导 · 演示动画
 * ============================================================ */
.demo-box { border: 1px solid #e2e8f0; border-radius: 10px; overflow: hidden; background: #fff; margin: 10px 0 18px; box-shadow: 0 3px 12px rgba(15,23,42,.05); }
.demo-bar { height: 26px; background: #f1f5f9; display: flex; align-items: center; gap: 5px; padding: 0 10px; border-bottom: 1px solid #e2e8f0; }
.demo-bar i { width: 8px; height: 8px; border-radius: 50%; display: block; }
.demo-bar i:nth-child(1) { background: #fca5a5; }
.demo-bar i:nth-child(2) { background: #fcd34d; }
.demo-bar i:nth-child(3) { background: #86efac; }
.demo-bar span { font-size: 11px; color: #94a3b8; margin-left: 6px; font-weight: 500; }

.demo-stage { position: relative; height: 152px; padding: 14px; background: #fbfdff; overflow: hidden; }
.demo-stage > * { position: absolute; left: 50%; transform: translateX(-50%); }

.dz { top: 16px; width: 170px; height: 46px; border: 1.5px dashed #cbd5e1; border-radius: 8px; display: flex; align-items: center; justify-content: center; gap: 6px; color: #94a3b8; font-size: 11px; background: #fff; animation: dzPulse 5s infinite; }
@keyframes dzPulse { 0%, 12% { border-color: #cbd5e1; background: #fff; } 18%, 30% { border-color: #1B3A5C; background: #eff6ff; } 40%, 100% { border-color: #cbd5e1; background: #fff; } }
.dz svg { width: 18px; height: 18px; }

.dz-file { top: 70px; font-size: 11px; color: #334155; background: #fff; border: 1px solid #e2e8f0; border-radius: 6px; padding: 3px 10px; opacity: 0; animation: fileIn 5s infinite; white-space: nowrap; }
@keyframes fileIn { 0%, 14% { opacity: 0; transform: translateX(-50%) translateY(8px) scale(.9); } 22%, 88% { opacity: 1; transform: translateX(-50%) translateY(0) scale(1); } 96%, 100% { opacity: 0; transform: translateX(-50%) translateY(0) scale(1); } }

.dz-bar { top: 100px; width: 170px; height: 4px; background: #e2e8f0; border-radius: 2px; overflow: hidden; opacity: 0; animation: barShow 5s infinite; }
.dz-bar i { display: block; height: 100%; width: 0; background: #1B3A5C; border-radius: 2px; animation: barFill 5s infinite; }
@keyframes barShow { 0%, 26% { opacity: 0; } 30%, 70% { opacity: 1; } 78%, 100% { opacity: 0; } }
@keyframes barFill { 0%, 28% { width: 0; } 68% { width: 100%; } 70%, 100% { width: 100%; } }

.dz-ok { top: 118px; font-size: 11px; color: #059669; font-weight: 500; opacity: 0; animation: okIn 5s infinite; white-space: nowrap; }
@keyframes okIn { 0%, 68% { opacity: 0; transform: translateX(-50%) translateY(4px); } 76%, 100% { opacity: 1; transform: translateX(-50%) translateY(0); } }

.demo-edit { display: flex; align-items: center; justify-content: center; gap: 12px; }
.edit-left { position: static; transform: none; flex: none; width: 150px; background: #fff; border: 1px solid #e2e8f0; border-radius: 8px; padding: 8px 10px; font-size: 11px; }
.edit-row { display: flex; align-items: center; gap: 6px; margin-bottom: 6px; }
.edit-row:last-child { margin-bottom: 0; }
.edit-label { color: #94a3b8; flex: none; width: 30px; }
.edit-input { flex: 1; border-bottom: 1px solid #cbd5e1; min-height: 16px; display: flex; align-items: center; overflow: hidden; }
.edit-typed { display: inline-block; white-space: nowrap; overflow: hidden; color: #1f2937; animation: typing 5s infinite; font-family: inherit; }
@keyframes typing { 0%, 8% { width: 0; } 40% { width: 3em; } 88% { width: 3em; } 96%, 100% { width: 0; } }
.edit-cursor { display: inline-block; width: 1.5px; height: 12px; background: #1B3A5C; animation: caret 1s steps(1) infinite; margin-left: 1px; }
@keyframes caret { 0%, 50% { opacity: 1; } 51%, 100% { opacity: 0; } }
.edit-arrow { position: static; transform: none; font-size: 18px; color: #cbd5e1; flex: none; }
.edit-right { position: static; transform: none; flex: none; width: 130px; background: #fff; border: 1px solid #e2e8f0; border-radius: 8px; padding: 10px; box-shadow: 0 2px 8px rgba(15,23,42,.06); }
.ep-name { font-size: 13px; font-weight: 700; color: #1B3A5C; letter-spacing: .5px; animation: epName 5s infinite; }
@keyframes epName { 0%, 36% { opacity: .25; } 46%, 90% { opacity: 1; } 96%, 100% { opacity: .25; } }
.ep-sub { font-size: 9px; color: #94a3b8; margin-top: 2px; }
.ep-line { height: 2px; background: #1B3A5C; opacity: .85; border-radius: 1px; margin: 6px 0 5px; }
.ep-item { height: 4px; background: #e2e8f0; border-radius: 2px; margin-bottom: 4px; }
.ep-item.short { width: 70%; }

.icon-demo-grid { position: static; transform: none; display: grid; grid-template-columns: repeat(8, 1fr); gap: 6px; width: 100%; max-width: 340px; margin: 0 auto; }
.icon-demo-cell { aspect-ratio: 1; border-radius: 6px; background: #f1f5f9; display: flex; align-items: center; justify-content: center; color: #64748b; position: relative; }
.icon-demo-cell svg { width: 62%; height: 62%; stroke: currentColor; fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
.icon-demo-cell.sel { background: #1B3A5C; color: #fff; }
.icon-cursor { position: absolute; width: 34px; height: 34px; border: 2px solid #1B3A5C; border-radius: 8px; pointer-events: none; animation: cursorMove 5s infinite; box-shadow: 0 0 0 3px rgba(27,58,92,.12); }
@keyframes cursorMove { 0%, 12% { transform: translate(0, 0); } 22%, 34% { transform: translate(42px, 0); } 44%, 56% { transform: translate(84px, 0); } 66%, 78% { transform: translate(126px, 0); } 88%, 100% { transform: translate(0, 0); } }

.demo-bullets { display: flex; flex-direction: column; gap: 8px; width: 100%; max-width: 300px; margin: 0 auto; justify-content: center; height: 100%; }
.bullet-line { display: flex; align-items: center; gap: 8px; font-size: 11.5px; color: #334155; }
.bullet-mark { width: 18px; text-align: right; font-weight: 700; color: #1B3A5C; flex: none; position: relative; height: 16px; }
.bullet-mark span { position: absolute; right: 0; top: 0; font-size: 11.5px; line-height: 16px; }
.bullet-mark .b-dot, .bullet-mark .b-circle, .bullet-mark .b-square, .bullet-mark .b-num, .bullet-mark .b-cn { opacity: 0; }
.bullet-mark .b-dot { color: #1B3A5C; animation: bCycle1 6s infinite; }
.bullet-mark .b-circle { color: #1B3A5C; animation: bCycle2 6s infinite; }
.bullet-mark .b-square { color: #1B3A5C; animation: bCycle3 6s infinite; }
.bullet-mark .b-num { animation: bCycle4 6s infinite; }
.bullet-mark .b-cn { animation: bCycle5 6s infinite; }
@keyframes bCycle1 { 0%,20%{opacity:1} 25%,100%{opacity:0} }
@keyframes bCycle2 { 0%,20%{opacity:0} 25%,40%{opacity:1} 45%,100%{opacity:0} }
@keyframes bCycle3 { 0%,40%{opacity:0} 45%,60%{opacity:1} 65%,100%{opacity:0} }
@keyframes bCycle4 { 0%,60%{opacity:0} 65%,80%{opacity:1} 85%,100%{opacity:0} }
@keyframes bCycle5 { 0%,80%{opacity:0} 85%,100%{opacity:1} }
.bullet-text { flex: 1; height: 6px; background: #e2e8f0; border-radius: 3px; }
.bullet-text.short { width: 70%; }

.demo-export { display: flex; align-items: center; justify-content: center; }
.exp-btn { position: static; transform: none; padding: 8px 18px; background: #1B3A5C; color: #fff; border-radius: 8px; font-size: 12px; font-weight: 600; box-shadow: 0 4px 12px rgba(27,58,92,.3); animation: btnPress 5s infinite; }
@keyframes btnPress { 0%, 10% { transform: scale(1); } 14%, 20% { transform: scale(.96); } 24%, 100% { transform: scale(1); } }
.exp-menu { position: absolute; top: 54px; left: 50%; transform: translateX(-50%) translateY(-6px) scale(.96); background: #fff; border: 1px solid #e2e8f0; border-radius: 10px; padding: 5px; box-shadow: 0 12px 30px rgba(15,23,42,.14); min-width: 180px; opacity: 0; animation: menuPop 5s infinite; }
@keyframes menuPop { 0%, 24% { opacity: 0; transform: translateX(-50%) translateY(-6px) scale(.96); } 32%, 82% { opacity: 1; transform: translateX(-50%) translateY(0) scale(1); } 92%, 100% { opacity: 0; transform: translateX(-50%) translateY(-6px) scale(.96); } }
.exp-item { display: flex; align-items: center; gap: 8px; padding: 7px 10px; border-radius: 7px; font-size: 12px; color: #334155; }
.exp-item .ico { width: 14px; height: 14px; border-radius: 3px; background: #e2e8f0; flex: none; }
.exp-item.active { background: #f1f6fc; color: #1B3A5C; font-weight: 600; }
.exp-item.active .ico { background: #1B3A5C; }

.demo-drafts { display: flex; flex-direction: column; gap: 8px; width: 100%; max-width: 320px; margin: 0 auto; justify-content: center; height: 100%; }
.draft-row { display: flex; align-items: center; gap: 8px; padding: 8px 10px; background: #fff; border: 1px solid #e2e8f0; border-radius: 8px; font-size: 11px; opacity: 0; animation: draftIn 6s infinite; }
.draft-row:nth-child(1) { animation-delay: 0s; }
.draft-row:nth-child(2) { animation-delay: .5s; }
.draft-row:nth-child(3) { animation-delay: 1s; }
@keyframes draftIn { 0% { opacity: 0; transform: translateX(-8px); } 10%, 80% { opacity: 1; transform: translateX(0); } 90%, 100% { opacity: 0; transform: translateX(-8px); } }
.draft-row .d-ico { width: 22px; height: 22px; border-radius: 5px; background: #eff6ff; color: #1B3A5C; display: flex; align-items: center; justify-content: center; flex: none; font-size: 12px; font-weight: 700; }
.draft-row .d-body { flex: 1; min-width: 0; }
.draft-row .d-t1 { height: 6px; width: 70%; background: #cbd5e1; border-radius: 3px; margin-bottom: 4px; }
.draft-row .d-t2 { height: 4px; width: 45%; background: #e2e8f0; border-radius: 2px; }
.draft-row .d-btn { padding: 3px 8px; background: #1B3A5C; color: #fff; border-radius: 5px; font-size: 10px; flex: none; }

/* ============================================================
 *  顶栏
 * ============================================================ */
.topbar{
  height:58px;display:flex;align-items:center;justify-content:space-between;
  padding:0 18px;background:#fff;border-bottom:1px solid #e5eaf1;
  position:relative;z-index:20;box-shadow:0 1px 3px rgba(15,23,42,.04);
}
.brand{font-weight:700;font-size:15px;letter-spacing:.5px}
.brand span{font-weight:400;color:#94a3b8;font-size:12px;margin-left:6px}
.top-actions{display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.chk{font-size:12px;color:#475569;display:flex;align-items:center;gap:4px;cursor:pointer;user-select:none}

.btn{
  border:1px solid #d7dee8;background:#fff;color:#334155;border-radius:8px;
  padding:7px 12px;font-size:13px;cursor:pointer;transition:.15s;
}
.btn:hover{background:#f8fafc;border-color:#c2cddc}
.btn.primary{background:#1B3A5C;border-color:#1B3A5C;color:#fff}
.btn.primary:hover{background:#15304d}
.btn.ghost{background:transparent}
.btn.tiny{padding:4px 9px;font-size:12px;border-radius:6px}

/* 缩放控制 */
.zoom-ctrl { display: flex; align-items: center; gap: 2px; background: #f1f5f9; border-radius: 8px; padding: 3px 4px; }
.zoom-ctrl button { width: 24px; height: 24px; border: none; background: #fff; border-radius: 5px; cursor: pointer; font-size: 14px; color: #334155; display: flex; align-items: center; justify-content: center; padding: 0; transition: .12s; font-family: inherit; line-height: 1; }
.zoom-ctrl button:hover { background: #1B3A5C; color: #fff; }
.zoom-ctrl input[type=number] { width: 44px; border: none; background: transparent; text-align: center; font-size: 12px; color: #1B3A5C; font-weight: 600; outline: none; padding: 2px 0; font-family: inherit; -moz-appearance: textfield; }
.zoom-ctrl input[type=number]::-webkit-outer-spin-button,
.zoom-ctrl input[type=number]::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
.zoom-ctrl .zoom-pct { font-size: 11px; color: #94a3b8; margin-right: 2px; }
.zoom-ctrl .zoom-fit { width: auto; padding: 0 8px; font-size: 11px; font-weight: 500; }

.export-wrap{position:relative}
.export-menu{
  position:absolute;top:calc(100% + 6px);right:0;background:#fff;border:1px solid #e5eaf1;
  border-radius:10px;box-shadow:0 12px 30px rgba(15,23,42,.14);padding:6px;min-width:210px;
  display:none;z-index:30;
}
.export-menu.show{display:block}
.export-menu button{
  display:block;width:100%;text-align:left;border:none;background:transparent;
  padding:9px 12px;border-radius:7px;font-size:13px;color:#334155;cursor:pointer;
}
.export-menu button:hover{background:#f1f6fc;color:#1B3A5C}

/* ============================================================
 *  布局
 * ============================================================ */
.app{display:flex;height:calc(100vh - 58px)}

.sidebar{
  width:392px;min-width:280px;max-width:640px;flex:none;
  background:#fff;border-right:1px solid #e5eaf1;
  overflow-y:auto;padding:14px;
}
.sidebar::-webkit-scrollbar{width:8px}
.sidebar::-webkit-scrollbar-thumb{background:#dbe3ec;border-radius:4px}

.splitter {
  width: 6px; flex: none; cursor: col-resize;
  background: #e2e8f0; position: relative;
  transition: background .15s; z-index: 10;
}
.splitter:hover, .splitter.dragging { background: #1B3A5C; }
.splitter::before {
  content: ''; position: absolute;
  top: 50%; left: 50%; transform: translate(-50%, -50%);
  width: 2px; height: 44px;
  background: rgba(148,163,184,.7); border-radius: 1px;
}
.splitter:hover::before, .splitter.dragging::before { background: rgba(255,255,255,.85); }

.stage{
  flex: 1; overflow: auto; padding: 24px;
  background: #eef1f6; display: flex; min-width: 0;
}
.stage::-webkit-scrollbar { width: 10px; height: 10px; }
.stage::-webkit-scrollbar-track { background: #eef1f6; }
.stage::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 5px; }
.stage::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

/* A4 纸张 */
.paper-wrap{
  --zoom: 1;
  position: relative; flex: none; margin: auto;
  width: calc(210mm * var(--zoom, 1));
  height: calc(297mm * var(--zoom, 1));
}
.paper{
  position: absolute; top: 0; left: 0;
  width: 210mm; height: 297mm;
  background: #fff; overflow: hidden;
  transform: scale(var(--zoom, 1));
  transform-origin: top left;
  box-shadow: 0 8px 30px rgba(15,23,42,.16);
  border-radius: 2px;
}
.page-content{position:absolute;inset:var(--page-pad,10mm);overflow:hidden}
.page-flow{--fit:1}

/* 头部 */
.rh{display:flex;align-items:flex-start;gap:12pt;justify-content:space-between}
.rh-left{flex:1;min-width:0}
.rh-name{margin:0;font-size:calc(var(--name-size,21) * 1pt * var(--fit));font-weight:700;letter-spacing:1.2pt;color:var(--primary);line-height:1.2;}
.rh-title{margin-top:calc(2.4 * var(--fit) * 1pt);font-size:calc(var(--contact-size,9) * 1.15 * 1pt * var(--fit));color:#5b6b7d;letter-spacing:.4pt;}
.rh-contacts{margin-top:calc(4 * var(--fit) * 1pt);display:flex;flex-wrap:wrap;gap:calc(3 * var(--fit) * 1pt) calc(12 * var(--fit) * 1pt);}
.ct{font-size:calc(var(--contact-size,9) * 1pt * var(--fit));color:#4a5a6b;display:inline-flex;align-items:center;gap:3pt;white-space:nowrap;}
.ct-i{display:inline-flex;align-items:center;justify-content:center;width:1.05em;height:1.05em;flex:none;color:var(--icon-color, var(--primary));}
.ct-i svg{width:100%;height:100%;display:block;stroke:currentColor;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round;}
.ct-i.emoji{width:auto;height:auto;font-size:1.02em;line-height:1;color:inherit;}
.photo{width:calc(var(--photo-w,72) * 1pt * var(--fit));aspect-ratio:3 / 4;height:auto;object-fit:cover;border-radius:3pt;flex:none;border:1px solid rgba(0,0,0,.08);display:block;}
.rh-line{height:calc(1.6 * var(--fit) * 1pt);background:var(--primary);opacity:.9;margin:calc(7 * var(--fit) * 1pt) 0 calc(8 * var(--fit) * 1pt);border-radius:2pt;}

/* 模块 */
.sec{margin:0}
.sec + .sec{border-top:calc(0.6 * var(--fit) * 1pt) solid var(--divider,#d3dbe4);margin-top:calc(var(--sec-gap,7) * 1pt * var(--fit));padding-top:calc(var(--sec-gap,7) * 1pt * var(--fit));}
.page-flow.no-divider .sec + .sec{border-top:none;margin-top:calc(var(--sec-gap,7) * 1pt * var(--fit));padding-top:0;}
.sec-title{display:flex;align-items:center;gap:calc(4 * var(--fit) * 1pt);margin:0 0 calc(4 * var(--fit) * 1pt);color:var(--primary);font-weight:700;line-height:1.35;font-size:calc(var(--sec-title-size,11) * 1pt * var(--fit));}
.sec-ico{display: inline-flex;align-items: center;justify-content: center;width: 1.15em;height: 1.15em;flex: none;color: var(--icon-color, var(--primary));}
.sec-ico svg {width: 100%; height: 100%; display: block;stroke: currentColor;fill: none;stroke-width: 1.8;stroke-linecap: round;stroke-linejoin: round;}
.sec-ico.emoji {font-size: .95em;line-height: 1;color: inherit;width: auto;height: auto;}
.ts-leftbar  .sec-title{padding-left:calc(6 * var(--fit) * 1pt);border-left:calc(2.4 * var(--fit) * 1pt) solid var(--primary)}
.ts-underline .sec-title{border-bottom:1.1pt solid var(--primary);padding-bottom:calc(1.6 * var(--fit) * 1pt)}
.ts-filled   .sec-title{background:var(--primary);color:#fff;padding:calc(2 * var(--fit) * 1pt) calc(7 * var(--fit) * 1pt);border-radius:3pt}
.ts-plain    .sec-title{letter-spacing:.6pt}
.ts-filled .sec-ico { color: #fff !important; }

.sec-body{font-size:calc(var(--sec-size,10) * 1pt * var(--fit));line-height:var(--lh,1.5);color:var(--text-color,#2f3640);}
.row2{display:flex;justify-content:space-between;align-items:baseline;gap:8pt}
.row2 .row-l{font-weight:600}
.row2.h2 .row-l{font-weight:500}
.row2 .row-r{color:#6b7a8c;font-size:.9em;white-space:nowrap;flex:none}
.p{margin:0 0 calc(1.6 * var(--fit) * 1pt)}

.li{display:flex;gap:calc(4 * var(--fit) * 1pt);margin-bottom:calc(1.8 * var(--fit) * 1pt)}
.li-t{flex:1;text-align:justify}
.dot{width:calc(3.8 * var(--fit) * 1pt);height:calc(3.8 * var(--fit) * 1pt);border-radius:50%;background:var(--bullet-color, var(--primary));flex:none;margin-top:.6em;}
.dot.dot-circle{background:transparent;border:calc(1 * var(--fit) * 1pt) solid var(--bullet-color, var(--primary));width:calc(3.6 * var(--fit) * 1pt);height:calc(3.6 * var(--fit) * 1pt);}
.dot.dot-square{border-radius:calc(0.5 * var(--fit) * 1pt);width:calc(3.2 * var(--fit) * 1pt);height:calc(3.2 * var(--fit) * 1pt);}
.bullet-num{flex:none;color:var(--bullet-color, var(--primary));font-weight:600;font-size:1em;line-height:inherit;min-width:calc(13 * var(--fit) * 1pt);text-align:right;font-variant-numeric:tabular-nums;}
.bullet-custom{font-weight:700}
.sp{height:calc(2.4 * var(--fit) * 1pt)}

.tbl {display: grid;column-gap: calc(8 * var(--fit) * 1pt);row-gap: calc(1.2 * var(--fit) * 1pt);}
.tbl .tr { display: contents; }
.tbl .tc {padding: calc(0.6 * var(--fit) * 1pt) 0;font-size: calc(var(--sec-size,10) * 1pt * var(--fit));line-height: var(--lh, 1.5);color: var(--text-color, #2f3640);word-break: break-word;}
.tbl .tc.bold { font-weight: 600; }
.tbl .tc.muted { color: #6b7a8c; }
.tbl .tc.center { text-align: center; }
.tbl .tc.right { text-align: right; }

/* ============================================================
 *  侧栏面板
 * ============================================================ */
.panel{border:1px solid #e8eef5;border-radius:12px;padding:12px;margin-bottom:12px;background:#fff}
.panel h3{margin:0 0 10px;font-size:13px;color:#0f172a;display:flex;align-items:center;justify-content:space-between;gap:8px;}
.inp{width:100%;border:1px solid #dde5ee;border-radius:8px;padding:7px 9px;font-size:13px;font-family:inherit;color:#1f2937;background:#fcfdfe;outline:none;transition:.15s;resize:vertical;}
.inp:focus{border-color:#93b4d8;background:#fff;box-shadow:0 0 0 3px rgba(27,58,92,.07)}
.inp:disabled{opacity:.45;cursor:not-allowed;background:#f7fafc}
textarea.inp{line-height:1.6;font-size:12.5px}
.hint{font-size:11.5px;color:#94a3b8;margin-top:6px;line-height:1.55}
.row{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
.fld{display:block;margin-bottom:8px;font-size:12px;color:#64748b}
.fld>span{display:block;margin-bottom:4px}
.fld .inp{font-size:13px;color:#1f2937}

.ct-row{display:flex;gap:6px;margin-bottom:4px;align-items:center}
.ct-row .ct-icon-btn{width:32px;height:32px;flex:none;border:1px solid #dde5ee;border-radius:8px;background:#fff;color:#1B3A5C;cursor:pointer;display:flex;align-items:center;justify-content:center;padding:0;transition:.15s;}
.ct-row .ct-icon-btn:hover{border-color:#93b4d8;background:#f1f6fc}
.ct-row .ct-icon-btn svg{width:16px;height:16px;stroke:currentColor;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round;}
.ct-row .ct-icon-btn .emoji{font-size:15px;line-height:1}
.ct-row .inp.ct-t{flex:1}
.ct-row .ct-del{width:30px;border:1px solid #dde5ee;border-radius:8px;background:#fff;color:#94a3b8;cursor:pointer;flex:none;}
.ct-row .ct-del:hover{color:#e11d48;border-color:#fecdd3;background:#fff5f6}

.ct-icon-panel{margin:-2px 0 8px 38px;padding:5px;background:#fbfdff;border:1px solid #e8eef5;border-radius:8px;}
.ct-icon-panel .icon-grid{grid-template-columns:repeat(8,1fr);margin-bottom:0;border:none;padding:0;background:transparent;max-height:104px;}

.photo-preview{display:flex;align-items:center;gap:10px;padding:8px;border:1px dashed #dbe3ec;border-radius:10px;background:#fbfdff;margin-bottom:8px;}
.photo-preview img{width:56px;height:74px;object-fit:cover;border-radius:5px;border:1px solid #e5eaf1;}
.photo-preview .ph-meta{flex:1;font-size:11.5px;color:#94a3b8;line-height:1.6}
.photo-preview .ph-meta b{color:#475569;display:block;font-size:12px;margin-bottom:2px}

.sec-card{border:1px solid #e8eef5;border-radius:10px;margin-bottom:10px;background:#fbfdff;overflow:hidden}
.sec-card-head{display:flex;align-items:center;gap:6px;padding:6px 8px;background:#f4f8fc;border-bottom:1px solid #e8eef5;}
.drag-idx{width:18px;height:18px;border-radius:50%;background:#dbe7f3;color:#3f5f80;font-size:11px;display:flex;align-items:center;justify-content:center;flex:none;}
.sec-name{flex:1;border:1px solid transparent;background:transparent;font-weight:600;font-size:13px;padding:4px 4px;border-radius:6px;outline:none;min-width:0;}
.sec-name:focus{background:#fff;border-color:#93b4d8}
.sec-actions{display:flex;gap:2px}
.sec-actions button{border:none;background:transparent;color:#94a3b8;cursor:pointer;font-size:13px;width:22px;height:22px;border-radius:5px;line-height:1;padding:0;}
.sec-actions button:hover{background:#e2ecf6;color:#1B3A5C}
.sec-card-body{padding:9px}
.grid3{display:grid;grid-template-columns:1fr 72px 72px;gap:8px;margin-bottom:8px}
.grid3 label{font-size:11.5px;color:#64748b;display:block}
.grid3 .inp{margin-top:3px}

.icon-picker-label {font-size: 11.5px; color: #64748b; margin-bottom: 4px;display: flex; align-items: center; justify-content: space-between;}
.icon-picker-label .cur-icon {display: inline-flex; align-items: center; gap: 4px;color: #1B3A5C; font-weight: 500;}
.icon-picker-label .cur-icon svg { width: 13px; height: 13px;stroke:currentColor;fill:none;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round;}

.icon-grid {display: grid;grid-template-columns: repeat(8, 1fr);gap: 4px;margin-bottom: 10px;padding: 5px;background: #fff;border: 1px solid #e8eef5;border-radius: 8px;max-height: 128px;overflow-y: auto;}
.icon-grid::-webkit-scrollbar { width: 6px; }
.icon-grid::-webkit-scrollbar-thumb { background: #dbe3ec; border-radius: 3px; }
.icon-grid button {aspect-ratio: 1;border: 1px solid transparent;background: transparent;border-radius: 6px;cursor: pointer;display: flex;align-items: center;justify-content: center;color: #64748b;padding: 5px;transition: .12s;}
.icon-grid button svg {width: 100%; height: 100%; display: block;stroke: currentColor; fill: none;stroke-width: 1.8;stroke-linecap: round;stroke-linejoin: round;}
.icon-grid button:hover {background: #f1f6fc;color: #1B3A5C;border-color: #dbe7f3;}
.icon-grid button.selected {background: #1B3A5C;color: #fff;border-color: #1B3A5C;}
.icon-grid button.none-btn svg { stroke-width: 1.6; }

.type-switch {display: flex; gap: 4px; margin-bottom: 8px;background: #f1f5f9; padding: 3px; border-radius: 8px;}
.type-switch button {flex: 1; border: none; background: transparent; padding: 5px 8px;font-size: 11.5px; color: #64748b; cursor: pointer; border-radius: 6px;transition: .15s;}
.type-switch button.active {background: #fff; color: #1B3A5C; font-weight: 600;box-shadow: 0 1px 3px rgba(15,23,42,.1);}

.tbl-editor { margin-top: 4px; }
.tbl-editor-head {display: flex; align-items: center; justify-content: space-between;font-size: 11.5px; color: #64748b; margin: 8px 0 4px;}
.tbl-editor-head button {border: 1px solid #dde5ee; background: #fff; padding: 2px 8px;border-radius: 5px; font-size: 11px; color: #1B3A5C; cursor: pointer;}
.tbl-editor-head button:hover { background: #f1f6fc; }

.tbl-col-row, .tbl-row {display: flex; gap: 4px; margin-bottom: 4px; align-items: center;}
.tbl-col-row input:not([type]),
.tbl-col-row input[type="text"] {flex: 1; border: 1px solid #dde5ee; border-radius: 6px; padding: 4px 6px;font-size: 11.5px; font-family: inherit; outline: none; background: #fcfdfe;min-width: 0;}
.tbl-col-row select {border: 1px solid #dde5ee; border-radius: 6px; padding: 4px 4px;font-size: 11.5px; font-family: inherit; outline: none; background: #fcfdfe;width: 48px; flex: none;}
.tbl-col-row input[type="number"] {border: 1px solid #dde5ee; border-radius: 6px; padding: 4px 4px;font-size: 11.5px; font-family: inherit; outline: none; background: #fcfdfe;width: 46px; flex: none;}
.tbl-col-row button, .tbl-row button {border: 1px solid #dde5ee; background: #fff; color: #94a3b8;width: 22px; height: 22px; border-radius: 5px; cursor: pointer;font-size: 11px; line-height: 1; padding: 0; flex: none;}
.tbl-col-row button:hover, .tbl-row button:hover {color: #e11d48; border-color: #fecdd3; background: #fff5f6;}
.tbl-row input {flex: 1; border: 1px solid #dde5ee; border-radius: 6px; padding: 4px 6px;font-size: 11.5px; font-family: inherit; outline: none; background: #fcfdfe;min-width: 0;}

/* ============================================================
 *  存档库 · 隐私保护
 * ============================================================ */
.privacy-note {
  padding: 12px 14px; background: #f0f7ff;
  border-left: 3px solid #1B3A5C; border-radius: 8px;
  font-size: 12.5px; line-height: 1.7; color: #334155;
  margin-bottom: 14px;
}
.privacy-note b { color: #1B3A5C; }
.privacy-note strong { color: #e11d48; font-weight: 600; }

.storage-mode {
  display: flex; flex-direction: column; gap: 8px;
  padding: 12px 14px; background: #f8fafc;
  border: 1px solid #e8eef5; border-radius: 10px;
  margin-bottom: 14px;
}
.storage-mode label {
  font-size: 12.5px; color: #334155; cursor: pointer;
  display: flex; align-items: center; gap: 8px; line-height: 1.55;
}
.storage-mode label input[type=radio] { margin: 0; flex: none; }

.draft-toolbar { display: flex; gap: 6px; margin-bottom: 12px; flex-wrap: wrap; }
.btn.danger { color: #e11d48; border-color: #fecdd3; }
.btn.danger:hover { background: #fff1f2; border-color: #e11d48; }

.draft-source {
  font-size: 10px; padding: 1px 6px; border-radius: 4px;
  background: #f1f5f9; color: #64748b; margin-left: 6px;
  font-weight: 500; vertical-align: middle;
}
.draft-source.local   { background: #ecfdf5; color: #059669; }
.draft-source.session { background: #fff7ed; color: #ea580c; }

.draft-item{display:flex;align-items:center;gap:10px;padding:10px 12px;border:1px solid #e8eef5;border-radius:10px;margin-bottom:8px;background:#fbfdff;transition:.15s;}
.draft-item:hover{border-color:#93b4d8;background:#f1f6fc}
.draft-item .d-info{flex:1;min-width:0}
.draft-item .d-name{font-weight:600;font-size:13px;color:#0f172a;margin-bottom:2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.draft-item .d-time{font-size:11px;color:#94a3b8}
.draft-item .d-actions{display:flex;gap:4px;flex:none}

/* ============================================================
 *  打印
 * ============================================================ */
@page{size:A4;margin:0}
@media print{
  html,body{height:auto !important;overflow:visible !important;background:#fff !important;margin:0 !important}
  .topbar,.sidebar,.splitter,.modal,.print-btn{display:none !important}
  .app{display:block !important;height:auto !important}
  .stage{display:block !important;padding:0 !important;background:#fff !important;overflow:visible !important;}
  .paper-wrap{
    --zoom: 1 !important;
    width:210mm !important;
    height:296.8mm !important;
    margin:0 auto !important;
    padding:0 !important;
    position:relative !important;
  }
  .paper{
    position:absolute !important;
    transform:none !important;
    top:0 !important;left:0 !important;
    width:210mm !important;
    height:296.8mm !important;
    box-shadow:none !important;
    border-radius:0 !important;
    page-break-after:avoid !important;
    page-break-inside:avoid !important;
  }
  *{-webkit-print-color-adjust:exact !important;print-color-adjust:exact !important}
}
</style>
</head>
<body>

<!-- ================= 顶栏 ================= -->
<div class="topbar">
  <div class="brand">📄 简历工坊<span>Resume Studio</span></div>
  <div class="top-actions">
    <button class="btn ghost" id="btnGuide">🎓 新手入门</button>
    <button class="btn ghost" id="btnDrafts">💾 存档库</button>
    <label class="chk"><input type="checkbox" id="autoFill" checked> 自动撑满一页</label>

    <div class="zoom-ctrl">
      <button id="zoomOut" title="缩小 (Ctrl/⌘ + 滚轮)">−</button>
      <input id="zoomInput" type="number" min="20" max="400" step="5" value="100" title="缩放百分比">
      <span class="zoom-pct">%</span>
      <button id="zoomIn" title="放大 (Ctrl/⌘ + 滚轮)">+</button>
      <button class="zoom-fit" id="zoomFit" title="适应窗口">适应</button>
    </div>

    <button class="btn ghost" id="btnReset">重置</button>
    <div class="export-wrap">
      <button class="btn primary" id="btnExport">⬇ 导出 ▾</button>
      <div class="export-menu" id="exportMenu">
        <button data-fmt="pdf">📄 PDF（浏览器打印）</button>
        <button data-fmt="doc">📝 Word 文档（.doc）</button>
        <button data-fmt="html">🌐 HTML 文件</button>
      </div>
    </div>
  </div>
</div>

<div class="app">
  <aside class="sidebar" id="sidebar">
    <div class="panel">
      <h3>① 导入简历</h3>
      <div class="row">
        <button class="btn" id="btnUpload">📥 上传文件</button>
        <button class="btn ghost" id="btnSample">载入示例</button>
        <input type="file" id="fileInput" accept=".docx,.pdf" hidden>
      </div>
      <div class="hint" style="margin-top:6px">支持 <b>Word (.docx)</b> 与 <b>PDF</b>，会自动识别版式与照片。</div>
      <div style="height:8px"></div>
      <textarea id="rawText" class="inp" rows="4" placeholder="也可以直接把简历纯文本粘贴到这里，再点下方按钮解析"></textarea>
      <div style="height:8px"></div>
      <button class="btn" id="btnParse" style="width:100%">✨ 智能解析为模块</button>
    </div>

    <div class="panel">
      <h3>② 头部信息</h3>
      <div class="photo-preview" id="photoPreview"></div>
      <div class="row" style="margin-bottom:10px">
        <button class="btn tiny" id="btnPhoto">📷 上传照片</button>
        <button class="btn tiny ghost" id="btnPhotoClear">清除照片</button>
        <input type="file" id="photoInput" accept="image/*" hidden>
      </div>
      <label class="fld">姓名<input class="inp" id="metaName"></label>
      <label class="fld">头衔 / 一句话简介<input class="inp" id="metaTitle"></label>
      <div class="fld"><span>联系方式（点击左侧图标可切换）</span><div id="contactList"></div></div>
      <button class="btn tiny" id="btnAddContact">+ 添加联系方式</button>
    </div>

    <div class="panel">
      <h3>③ 内容模块 <button class="btn tiny" id="btnAddSection">+ 新增模块</button></h3>
      <div id="secList"></div>
    </div>

    <div class="panel">
      <h3>④ 全局样式</h3>
      <div class="row" style="margin-bottom:10px">
        <label class="chk"><input type="checkbox" id="thDivider" checked> 模块之间显示横线</label>
      </div>
      <div class="grid3">
        <label>主题色<input type="color" class="inp" id="thPrimary" style="padding:2px;height:32px"></label>
        <label>姓名号<input type="number" step="0.5" class="inp" id="thNameSizeBase"></label>
        <label>行高<input type="number" step="0.02" class="inp" id="thLh"></label>
      </div>
      <div class="grid3">
        <label>标题风格
          <select class="inp" id="thTitleStyle">
            <option value="plain">纯文字</option>
            <option value="leftbar">左侧竖条</option>
            <option value="underline">下划线</option>
            <option value="filled">填充色块</option>
          </select>
        </label>
        <label>页边距<input type="number" step="0.5" class="inp" id="thPad"></label>
        <label>模块间距<input type="number" step="0.5" class="inp" id="thGap"></label>
      </div>
      <div class="grid3">
        <label>分隔线色<input type="color" class="inp" id="thDividerColor" style="padding:2px;height:32px"></label>
        <label>正文字号<input type="number" step="0.5" class="inp" id="thBaseSize"></label>
        <label>照片宽<input type="number" step="2" class="inp" id="thPhotoW"></label>
      </div>
      <div class="grid3">
        <label>图标色<input type="color" class="inp" id="thIconColor" style="padding:2px;height:32px" title="标题图标 + 联系方式图标"></label>
        <label style="grid-column:span 2;align-self:flex-end">
          <span class="chk" style="font-size:11.5px"><input type="checkbox" id="thIconFollow"> 跟随主题色</span>
        </label>
      </div>
      <div class="grid3">
        <label>项目符号
          <select class="inp" id="thBulletStyle">
            <option value="dot">● 实心圆点</option>
            <option value="circle">○ 空心圆</option>
            <option value="square">■ 实心方块</option>
            <option value="dash">— 短横线</option>
            <option value="number">1. 阿拉伯数字</option>
            <option value="number-paren">1) 数字加括号</option>
            <option value="number-cn">一、中文数字</option>
            <option value="letter">a. 小写字母</option>
            <option value="custom">✱ 自定义</option>
            <option value="none">无符号</option>
          </select>
        </label>
        <label>自定义<input class="inp" id="thBulletChar" maxlength="3" placeholder="◆"></label>
        <label>符号色<input type="color" class="inp" id="thBulletColor" style="padding:2px;height:32px"></label>
      </div>
      <label class="fld">字体
        <select class="inp" id="thFont">
          <option value='"PingFang SC","Microsoft YaHei","Hiragino Sans GB",sans-serif'>苹方 / 微软雅黑</option>
          <option value='"Songti SC","SimSun",serif'>宋体（衬线）</option>
          <option value='"Kaiti SC","KaiTi",serif'>楷体</option>
          <option value='"Source Han Sans CN","Noto Sans SC",sans-serif'>思源黑体</option>
        </select>
      </label>
      <div class="hint">语法：<b># 主标题 | 右侧信息</b>　<b>- 项目符号</b>　<b>**加粗**</b></div>
    </div>
  </aside>

  <div class="splitter" id="splitter" title="拖动调整左侧栏宽度"></div>

  <main class="stage" id="stage">
    <div class="paper-wrap" id="paperWrap">
      <div class="paper" id="paper">
        <div class="page-content" id="pageContent">
          <div class="page-flow" id="pageFlow"></div>
        </div>
      </div>
    </div>
  </main>
</div>

<!-- ================= 新手引导 ================= -->
<div class="modal" id="guideModal">
  <div class="modal-box">
    <div class="modal-head">
      <h2>🎓 新手入门 · 6 步做出专业简历</h2>
      <button class="modal-x" data-close>×</button>
    </div>
    <div class="modal-body">

      <h3>⓪ 界面缩放与宽度调节</h3>
      <p>顶栏中间的缩放控制可以调整右侧预览的显示大小，左侧栏与预览区之间的分隔条可以拖动调整宽度。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>拖动分隔条 + 缩放预览</span></div>
        <div class="demo-stage">
          <div class="demo-edit">
            <div class="edit-left" style="width:100px">
              <div class="edit-row"><span class="edit-label" style="width:auto;color:#1B3A5C;font-weight:600">左侧栏</span></div>
              <div style="height:6px;background:#e2e8f0;border-radius:3px;margin-top:6px"></div>
              <div style="height:6px;background:#e2e8f0;border-radius:3px;margin-top:5px;width:80%"></div>
              <div style="height:6px;background:#e2e8f0;border-radius:3px;margin-top:5px;width:60%"></div>
            </div>
            <div class="edit-arrow">↔</div>
            <div class="edit-right" style="width:150px;height:88px;padding:8px">
              <div style="font-size:9px;color:#94a3b8;text-align:center">预览缩放</div>
              <div style="font-size:20px;font-weight:700;color:#1B3A5C;text-align:center;margin-top:4px">100%</div>
              <div style="display:flex;gap:4px;justify-content:center;margin-top:6px">
                <span style="display:inline-block;width:20px;height:20px;background:#f1f5f9;border-radius:4px;text-align:center;line-height:20px;font-size:12px;font-weight:700;color:#334155">−</span>
                <span style="display:inline-block;width:20px;height:20px;background:#f1f5f9;border-radius:4px;text-align:center;line-height:20px;font-size:12px;font-weight:700;color:#334155">+</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <ul>
        <li>顶栏缩放控件：<b>−</b> / <b>+</b> 按钮逐级调整，输入框可直接输入百分比，<b>适应</b>按钮回到自动适配</li>
        <li>快捷键：<b>Ctrl / ⌘ + 鼠标滚轮</b> 在预览区快速缩放</li>
        <li>拖动左侧栏与预览区之间的<b>竖直分隔条</b>可调整左栏宽度</li>
      </ul>

      <h3>① 导入 / 载入简历</h3>
      <p>点击左侧「<b>上传文件</b>」支持 <b>Word (.docx)</b> 与 <b>PDF</b>；也可以把简历文字直接粘贴到文本框后点「智能解析」。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>上传简历 → 自动解析</span></div>
        <div class="demo-stage">
          <div class="dz"><svg viewBox="0 0 24 24"><path d="M12 16V4M6 10l6-6 6 6M4 20h16"/></svg><span>拖入 .docx / .pdf</span></div>
          <div class="dz-file">📄 小猫咪-简历.docx</div>
          <div class="dz-bar"><i></i></div>
          <div class="dz-ok">✓ 识别 6 个模块 · 已提取照片</div>
        </div>
      </div>
      <ul>
        <li>Word 会自动提取文档里的第一张图片作为证件照</li>
        <li>PDF 会按坐标识别文字位置，自动判断左右分栏 / 左中右版式</li>
        <li>没想好？点「载入示例」即可看到一份完整样例</li>
      </ul>

      <h3>② 编辑内容</h3>
      <p>左侧所有输入都是实时的，右侧 A4 预览会立刻同步更新。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>输入姓名 → 右侧预览同步更新</span></div>
        <div class="demo-stage demo-edit">
          <div class="edit-left">
            <div class="edit-row"><span class="edit-label">姓名</span><span class="edit-input"><span class="edit-typed">小猫咪</span><span class="edit-cursor"></span></span></div>
            <div class="edit-row"><span class="edit-label">头衔</span><span class="edit-input"><span style="color:#94a3b8">硕士研究生</span></span></div>
          </div>
          <div class="edit-arrow">→</div>
          <div class="edit-right">
            <div class="ep-name">小猫咪</div>
            <div class="ep-sub">2027 届硕士研究生</div>
            <div class="ep-line"></div>
            <div class="ep-item"></div>
            <div class="ep-item short"></div>
          </div>
        </div>
      </div>
      <ul>
        <li><b>头部信息</b>：姓名、头衔、联系方式（可增删行）</li>
        <li><b>内容模块</b>：每个模块可以改名称、图标、字号，还能上移 / 下移 / 删除</li>
      </ul>

      <h3>③ 扁平线条图标</h3>
      <p>标题图标和联系方式图标都是 <b>扁平简约线条图标</b>，可以自由切换，颜色也能自定义。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>点击图标 → 高亮选中 → 标题同步变化</span></div>
        <div class="demo-stage" style="display:flex;align-items:center;justify-content:center">
          <div class="icon-demo-grid">
            <div class="icon-demo-cell sel"><svg viewBox="0 0 24 24"><path d="M22 10 12 5 2 10l10 5 10-5Z"/><path d="M6 11.5V17c0 1.5 2.7 3 6 3s6-1.5 6-3v-5.5"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><rect x="3" y="7" width="18" height="13" rx="2"/><path d="M9 7V5a2 2 0 0 1 2-2h2a2 2 0 0 1 2 2v2"/><path d="M3 12h18"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><path d="M5 13c0-5 3-8.5 7-10.5 4 2 7 5.5 7 10.5l-2.5 2.5H7.5Z"/><circle cx="12" cy="10" r="1.8"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><path d="M8 4h8v5a4 4 0 0 1-8 0V4Z"/><path d="M12 13v4M9 20h6"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><path d="M14.5 6.5a3.7 3.7 0 0 0 5 5l-8.5 8.5a2 2 0 0 1-3-3L16.5 8.5a3.7 3.7 0 0 0-5-5Z"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9.5"/><path d="M2.5 12h19"/><path d="M12 2.5a15 15 0 0 1 0 19 15 15 0 0 1 0-19Z"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><path d="M3 3v18h18"/><path d="M7 14v4M12 9v9M17 5v13"/></svg></div>
            <div class="icon-demo-cell"><svg viewBox="0 0 24 24"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg></div>
          </div>
          <div class="icon-cursor"></div>
        </div>
      </div>
      <ul>
        <li>点击联系方式行左侧方形图标按钮，会展开图标面板，自由切换</li>
        <li>图标颜色可在「全局样式」统一设置，或勾选「跟随主题色」</li>
        <li>「填充色块」标题风格下，标题图标会自动变成白色以保持对比度</li>
      </ul>

      <h3>④ 项目符号自定义</h3>
      <p>正文中所有 <code>- 文本</code> 条目都会套用同一个项目符号样式，可随时切换。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>项目符号循环切换：● ○ ■ 1. 一、</span></div>
        <div class="demo-stage">
          <div class="demo-bullets">
            <div class="bullet-line"><span class="bullet-mark"><span class="b-dot">●</span><span class="b-circle">○</span><span class="b-square">■</span><span class="b-num">1.</span><span class="b-cn">一、</span></span><span class="bullet-text"></span></div>
            <div class="bullet-line"><span class="bullet-mark"><span class="b-dot">●</span><span class="b-circle">○</span><span class="b-square">■</span><span class="b-num">2.</span><span class="b-cn">二、</span></span><span class="bullet-text short"></span></div>
            <div class="bullet-line"><span class="bullet-mark"><span class="b-dot">●</span><span class="b-circle">○</span><span class="b-square">■</span><span class="b-num">3.</span><span class="b-cn">三、</span></span><span class="bullet-text"></span></div>
          </div>
        </div>
      </div>
      <ul>
        <li>「全局样式」中可切换 10 种符号：实心圆点 / 空心圆 / 实心方块 / 短横线 / 阿拉伯数字 / 数字加括号 / 中文数字 / 小写字母 / 自定义 / 无符号</li>
        <li>选「自定义」时可输入任意符号（如 ◆ ▪ → ★ 等）</li>
        <li>「符号色」可单独控制项目符号颜色，与图标色相互独立</li>
      </ul>

      <h3>⑤ 表格排版</h3>
      <p>每个模块支持「📝 文本 / 📊 表格」两种模式，切换到表格模式后可以自定义列名、对齐方式和宽度比例。</p>

      <h3>⑥ 存档与导出</h3>
      <p>所有编辑都可以保存为草稿，随时加载；完成后可导出为 PDF / Word / HTML 三种格式。</p>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>存档库 · 多版本草稿一键加载</span></div>
        <div class="demo-stage">
          <div class="demo-drafts">
            <div class="draft-row"><span class="d-ico">1</span><span class="d-body"><span class="d-t1" style="display:block"></span><span class="d-t2" style="display:block"></span></span><span class="d-btn">加载</span></div>
            <div class="draft-row"><span class="d-ico">2</span><span class="d-body"><span class="d-t1" style="display:block"></span><span class="d-t2" style="display:block"></span></span><span class="d-btn">加载</span></div>
            <div class="draft-row"><span class="d-ico">3</span><span class="d-body"><span class="d-t1" style="display:block"></span><span class="d-t2" style="display:block"></span></span><span class="d-btn">加载</span></div>
          </div>
        </div>
      </div>
      <div class="demo-box">
        <div class="demo-bar"><i></i><i></i><i></i><span>导出菜单 · PDF / Word / HTML</span></div>
        <div class="demo-stage demo-export">
          <div class="exp-btn">⬇ 导出 ▾</div>
          <div class="exp-menu">
            <div class="exp-item active"><span class="ico"></span>PDF（浏览器打印）</div>
            <div class="exp-item"><span class="ico"></span>Word 文档（.doc）</div>
            <div class="exp-item"><span class="ico"></span>HTML 文件</div>
          </div>
        </div>
      </div>
      <ul>
        <li>点顶栏「<b>💾 存档库</b>」可以保存 / 加载 / 重命名 / 删除多份草稿</li>
        <li>点顶栏「<b>⬇ 导出</b>」可选择 PDF / Word / HTML 三种格式</li>
        <li>Word 导出时线条图标和项目符号会自动转为兼容格式</li>
      </ul>

      <p style="margin-top:14px;padding:10px 12px;background:#f0f7ff;border-left:3px solid #1B3A5C;border-radius:6px;color:#1B3A5C">
        💡 <b>小提示</b>：页面缩放会自动把内容撑满一页；若内容太多导致字号被压得太小，可以精简文字或调小「模块间距 / 页边距」。
      </p>
    </div>
  </div>
</div>

<!-- ================= 存档库 ================= -->
<div class="modal" id="draftModal">
  <div class="modal-box">
    <div class="modal-head">
      <h2>💾 存档库</h2>
      <button class="modal-x" data-close>×</button>
    </div>
    <div class="modal-body">

      <div class="privacy-note">
        🔒 <b>隐私说明</b>：所有草稿<strong>仅保存在你当前浏览器的本地存储</strong>中，
        不会上传到任何服务器，其他人访问此链接<strong>无法看到你的草稿</strong>。
        但换设备或清理浏览器数据后草稿会丢失，建议定期使用「导出全部备份」。
      </div>

      <div class="storage-mode">
        <div style="font-size:12px;color:#64748b;margin-bottom:2px">保存位置（影响新保存的草稿）</div>
        <label><input type="radio" name="storageMode" value="local" checked>
          💾 <b>本地持久存储</b>（推荐）— 关浏览器不丢，适合个人电脑</label>
        <label><input type="radio" name="storageMode" value="session">
          🔐 <b>会话临时存储</b> — 关标签页自动清空，适合公共电脑</label>
      </div>

      <div style="display:flex;gap:10px;margin-bottom:12px">
        <input class="inp" id="newDraftName" placeholder="给这份草稿起个名字（例如：投互联网运营岗）" style="flex:1">
        <button class="btn primary" id="btnSaveDraft">保存当前</button>
      </div>

      <div class="draft-toolbar">
        <button class="btn tiny" id="btnExportDrafts">📤 导出全部备份</button>
        <button class="btn tiny" id="btnImportDrafts">📥 导入备份</button>
        <button class="btn tiny danger" id="btnClearAllDrafts">🗑 清空全部</button>
        <input type="file" id="importDraftFile" accept=".json" hidden>
      </div>

      <div id="draftList"></div>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/mammoth@1.8.0/mammoth.browser.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pdfjs-dist@3.11.174/build/pdf.min.js"></script>

<script>
/* ============================================================
 *  0. 工具
 * ============================================================ */
const $  = (s, r = document) => r.querySelector(s);
const $$ = (s, r = document) => [...r.querySelectorAll(s)];
let _uid = 0;
const uid = () => 's' + (++_uid) + '_' + Date.now().toString(36);
const esc = s => String(s ?? '').replace(/[&<>"]/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const debounce = (fn, ms = 160) => { let t; return (...a) => { clearTimeout(t); t = setTimeout(() => fn(...a), ms); }; };

if (window.pdfjsLib) {
  pdfjsLib.GlobalWorkerOptions.workerSrc =
    'https://cdn.jsdelivr.net/npm/pdfjs-dist@3.11.174/build/pdf.worker.min.js';
}

function toCnNum(n) {
  if (n < 1) return String(n);
  const d = '一二三四五六七八九';
  if (n < 10) return d[n - 1];
  if (n === 10) return '十';
  if (n < 20) return '十' + d[n - 11];
  if (n < 100) {
    const t = Math.floor(n / 10), o = n % 10;
    return d[t - 1] + '十' + (o > 0 ? d[o - 1] : '');
  }
  return String(n);
}

/* ============================================================
 *  1. 图标库
 * ============================================================ */
const ICON_LIB = {
  edu:      { name: '教育',   svg: '<path d="M22 10 12 5 2 10l10 5 10-5Z"/><path d="M6 11.5V17c0 1.5 2.7 3 6 3s6-1.5 6-3v-5.5"/>' },
  work:     { name: '工作',   svg: '<rect x="3" y="7" width="18" height="13" rx="2"/><path d="M9 7V5a2 2 0 0 1 2-2h2a2 2 0 0 1 2 2v2"/><path d="M3 12h18"/>' },
  building: { name: '公司',   svg: '<rect x="4" y="3" width="16" height="18" rx="1.5"/><path d="M9 21v-3.5h6V21"/><path d="M8 7h.01M12 7h.01M16 7h.01M8 11h.01M12 11h.01M16 11h.01M8 15h.01M16 15h.01"/>' },
  rocket:   { name: '项目',   svg: '<path d="M5 13c0-5 3-8.5 7-10.5 4 2 7 5.5 7 10.5l-2.5 2.5H7.5Z"/><circle cx="12" cy="10" r="1.8"/><path d="M8.5 16 6 20M15.5 16l2.5 4"/>' },
  trophy:   { name: '荣誉',   svg: '<path d="M8 4h8v5a4 4 0 0 1-8 0V4Z"/><path d="M8 6H6a2 2 0 0 0 2 2M16 6h2a2 2 0 0 1-2 2"/><path d="M12 13v4M9 20h6"/>' },
  award:    { name: '奖章',   svg: '<circle cx="12" cy="8" r="5.5"/><path d="M8.5 12.5 7 21l5-3 5 3-1.5-8.5"/>' },
  wrench:   { name: '技能',   svg: '<path d="M14.5 6.5a3.7 3.7 0 0 0 5 5l-8.5 8.5a2 2 0 0 1-3-3L16.5 8.5a3.7 3.7 0 0 0-5-5Z"/>' },
  bulb:     { name: '亮点',   svg: '<path d="M9 18h6M10 22h4"/><path d="M12 2a7 7 0 0 0-4 12.5V17h8v-2.5A7 7 0 0 0 12 2Z"/>' },
  book:     { name: '学术',   svg: '<path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2Z"/>' },
  globe:    { name: '语言',   svg: '<circle cx="12" cy="12" r="9.5"/><path d="M2.5 12h19"/><path d="M12 2.5a15 15 0 0 1 0 19 15 15 0 0 1 0-19Z"/>' },
  users:    { name: '团队',   svg: '<path d="M16 20v-1.5a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4V20"/><circle cx="9" cy="7" r="4"/><path d="M22 20v-1.5a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/>' },
  chart:    { name: '数据',   svg: '<path d="M3 3v18h18"/><path d="M7 14v4M12 9v9M17 5v13"/>' },
  target:   { name: '目标',   svg: '<circle cx="12" cy="12" r="9.5"/><circle cx="12" cy="12" r="5.5"/><circle cx="12" cy="12" r="1.5"/>' },
  layers:   { name: '层级',   svg: '<path d="M12 2 2 7l10 5 10-5-10-5Z"/><path d="M2 17l10 5 10-5"/><path d="M2 12l10 5 10-5"/>' },
  palette:  { name: '兴趣',   svg: '<circle cx="12" cy="12" r="9.5"/><circle cx="8.5" cy="10" r="1"/><circle cx="15.5" cy="10" r="1"/><circle cx="12" cy="7" r="1"/><circle cx="12" cy="16.5" r="1"/>' },
  link:     { name: '链接',   svg: '<path d="M10 13a5 5 0 0 0 7.5.5l3-3a5 5 0 0 0-7-7L11.8 5.2"/><path d="M14 11a5 5 0 0 0-7.5-.5l-3 3a5 5 0 0 0 7 7l1.7-1.7"/>' },
  phone:    { name: '电话',   svg: '<path d="M21.5 16.9v3a2 2 0 0 1-2.2 2 19.8 19.8 0 0 1-8.6-3.1 19.5 19.5 0 0 1-6-6A19.8 19.8 0 0 1 1.6 4.2 2 2 0 0 1 3.6 2h3a2 2 0 0 1 2 1.7 12.8 12.8 0 0 0 .7 2.8 2 2 0 0 1-.5 2.1L7.6 9.9a16 16 0 0 0 6 6l1.3-1.3a2 2 0 0 1 2.1-.4 12.8 12.8 0 0 0 2.8.7 2 2 0 0 1 1.7 2Z"/>' },
  mail:     { name: '邮箱',   svg: '<rect x="2" y="4.5" width="20" height="15" rx="2"/><path d="m2.5 7 9.5 6.5L21.5 7"/>' },
  pin:      { name: '地址',   svg: '<path d="M12 22s7-6.3 7-12a7 7 0 1 0-14 0c0 5.7 7 12 7 12Z"/><circle cx="12" cy="10" r="2.5"/>' },
  wechat:   { name: '微信',   svg: '<path d="M8.5 4C4.9 4 2 6.5 2 9.6c0 1.7 1 3.3 2.5 4.4L4 16.5l2.6-1.3c.6.2 1.2.3 1.9.3"/><path d="M22 14.4c0-2.7-2.5-4.9-5.6-4.9s-5.6 2.2-5.6 4.9 2.5 4.9 5.6 4.9c.6 0 1.1-.1 1.6-.2l2.2 1.1-.5-1.9c1.4-.9 2.3-2.4 2.3-3.9Z"/>' },
  file:     { name: '文档',   svg: '<path d="M14 2H6.5A2.5 2.5 0 0 0 4 4.5v15A2.5 2.5 0 0 0 6.5 22h11a2.5 2.5 0 0 0 2.5-2.5V8Z"/><path d="M14 2v6h6M15 13H9M15 17H9M11 9H9"/>' },
  code:     { name: '技术',   svg: '<polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/>' },
  calendar: { name: '时间',   svg: '<rect x="3" y="4.5" width="18" height="17" rx="2"/><path d="M16 2.5v4M8 2.5v4M3 10h18"/>' },
  mic:      { name: '主持',   svg: '<path d="M12 1.5a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0v-8a3 3 0 0 0-3-3Z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2M12 19v4M8 23h8"/>' },
  pencil:   { name: '写作',   svg: '<path d="M17 3a2.85 2.83 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5Z"/>' },
  compass:  { name: '方向',   svg: '<circle cx="12" cy="12" r="9.5"/><path d="m16.2 7.8-2.1 6.4-6.3 2 2.1-6.4 6.3-2Z"/>' },
  message:  { name: '沟通',   svg: '<path d="M21 14.5a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2Z"/>' },
  grid:     { name: '综合',   svg: '<rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/>' },
  heart:    { name: '志愿',   svg: '<path d="M20.8 4.6a5.5 5.5 0 0 0-7.8 0L12 5.7l-1-1.1a5.5 5.5 0 0 0-7.8 7.8l1 1L12 21.2l7.8-7.8 1-1a5.5 5.5 0 0 0 0-7.8Z"/>' },
  none:     { name: '无图标', svg: '<circle cx="12" cy="12" r="9.5"/><path d="m5 5 14 14"/>' }
};

const ICON_EMOJI_MAP = {
  edu: '🎓', work: '💼', building: '🏢', rocket: '🚀',
  trophy: '🏆', award: '🏅', wrench: '🛠', bulb: '💡',
  book: '📚', globe: '🌐', users: '👥', chart: '📊',
  target: '🎯', layers: '📚', palette: '🎨', link: '🔗',
  phone: '📱', mail: '📧', pin: '📍', wechat: '💬',
  file: '📄', code: '💻', calendar: '📅', mic: '🎤',
  pencil: '✏', compass: '🧭', message: '💬', grid: '▦',
  heart: '❤', none: ''
};

const isIconKey = v => v && Object.prototype.hasOwnProperty.call(ICON_LIB, v) && v !== 'none';

function renderIconHtml(iconKey, colorOverride) {
  if (!iconKey) return '';
  if (isIconKey(iconKey)) {
    const color = colorOverride || 'var(--icon-color, var(--primary))';
    return `<span class="sec-ico" style="color:${color}"><svg viewBox="0 0 24 24" aria-hidden="true">${ICON_LIB[iconKey].svg}</svg></span>`;
  }
  return `<span class="sec-ico emoji">${esc(iconKey)}</span>`;
}

function renderContactIcon(iconKey, colorOverride) {
  if (!iconKey) return '';
  if (isIconKey(iconKey)) {
    const color = colorOverride || 'var(--icon-color, var(--primary))';
    return `<span class="ct-i" style="color:${color}"><svg viewBox="0 0 24 24" aria-hidden="true">${ICON_LIB[iconKey].svg}</svg></span>`;
  }
  return `<span class="ct-i emoji">${esc(iconKey)}</span>`;
}

/* ============================================================
 *  2. 示例数据（小猫咪版）
 * ============================================================ */
function sampleState() {
  return {
    meta: {
      name: '小猫咪',
      title: '2027 届硕士研究生 · 人类管理方向',
      photo: '',
      contacts: [
        { icon: 'mail',  text: 'meow@example.com' },
        { icon: 'pin',   text: '现所在地：地球' },
        { icon: 'phone', text: '13xxxxxxxxx' }
      ]
    },
    theme: {
      primary: '#1B3A5C', textColor: '#2f3640', divider: '#d3dbe4', dividerOn: true,
      font: '"PingFang SC","Microsoft YaHei","Hiragino Sans GB",sans-serif',
      lineHeight: 1.45, pagePad: 10, sectionGap: 6, titleStyle: 'plain',
      nameSize: 20, contactSize: 8.5, baseSize: 9.5, photoW: 72,
      iconColor: '#1B3A5C', iconFollow: true,
      bulletStyle: 'dot', bulletChar: '◆', bulletColor: '#1B3A5C'
    },
    sections: [
      {
        id: uid(), name: '教育背景', icon: 'edu', size: 9.5, titleSize: 10.5, type: 'table',
        columns: [
          { name: '学校名称', align: 'left',   weight: 3,   bold: true },
          { name: '专业',     align: 'left',   weight: 3,   bold: false },
          { name: '平均绩点', align: 'center', weight: 1.5, bold: false },
          { name: '时间',     align: 'right',  weight: 2.5, bold: false, muted: true }
        ],
        rows: [
          ['猫咪大学', '地球占领与治理（硕士）', '3.92 / 4.0', '2024.09 - 2027.06'],
          ['猫咪大学', '人类语言文学（本科）',   '3.85 / 4.0', '2020.09 - 2024.06']
        ]
      },
      {
        id: uid(), name: '实习经历', icon: 'work', size: 9.5, titleSize: 10.5, type: 'text',
        content:
`# 拜见猫咪大王有限公司 · 产品运营实习生 | 2024.09 - 2026.03
- 用户增长体系搭建：从 0 到 1 搭建「公众号 + 小红书 + 抖音」内容矩阵，主导选题、脚本与投放节奏，矩阵累计曝光 320 万+，单篇笔记最高点赞 8000+。
- 数据驱动迭代：建立周度数据看板，跟踪曝光、点击、留存、转化四层指标，通过 A/B 测试优化落地页文案与投放素材，注册转化率从 8% 提升至 17.6%。
- 用户研究与需求洞察：每月整理 200+ 条用户反馈与评论关键词，输出需求洞察报告 6 份，其中 3 项建议被产品团队采纳并落地。
- 跨部门协同：与产品、设计、研发保持双周例会节奏，牵头推动 12 个运营活动按节点上线，跨部门协作效率提升约 40%。
# 猫咪娱乐有限公司 · 运营实习生 | 2023.07 - 2023.09
- 内容运营：负责官方公众号选题、撰写与排版，单篇平均阅读量提升 35%，粉丝净增 1800+。
- 活动策划：参与主题营销活动，配合搭建社群承接流量，活动期社群转化率达 22%。`
      },
      {
        id: uid(), name: '项目经历', icon: 'rocket', size: 9.5, titleSize: 10.5, type: 'text',
        content:
`# 「城市漫游指南」校园生活服务小程序 | 2025.03 - 2025.08
- 需求调研与定位：访谈 60+ 名在校学生，梳理出「路线推荐 / 优惠聚合 / 打卡分享」三大核心需求，确定产品定位与 MVP 功能范围。
- 内容冷启动：牵头搭建内容池，组织 20 位校园 KOC 共创首批 150 条路线笔记，上线首月 DAU 突破 1200。
- 增长与留存：设计「打卡集章」激励机制，配合社群运营，7 日留存率达 41%，累计注册用户 5200+。
# 「毕业不慌」求职经验分享活动 | 2024.11 - 2024.12
- 活动策划：联合 5 个学院社团，策划 3 场线上分享 + 1 场线下沙龙，累计报名 800+ 人。
- 内容运营：整理嘉宾干货为图文长帖，活动期间公众号新增关注 1500+。
- 转化承接：搭建 3 个求职交流社群，累计沉淀精准用户 600+。`
      },
      {
        id: uid(), name: '校园经历', icon: 'users', size: 9.5, titleSize: 10.5, type: 'text',
        content:
`# 猫咪大学研究生会 · 宣传部副部长 | 2024.09 - 2025.06
- 统筹校内 20+ 场活动的宣传工作，管理 8 人内容团队，产出图文 120+ 篇、视频 30+ 条。
# 猫咪大学 · 校广播台主持人 | 2021.09 - 2024.06
- 主持校级晚会、讲座等活动 40+ 场，具备良好的表达能力与临场应变能力。`
      },
      {
        id: uid(), name: '技能与证书', icon: 'wrench', size: 9.5, titleSize: 10.5, type: 'text',
        content:
`# 内容与增长
- 熟悉小红书 / 抖音 / 公众号内容生态与算法逻辑，具备账号矩阵搭建、选题策划、数据复盘完整能力。
# 数据分析
- 熟练使用 Excel 数据透视表与函数，能用 SQL 完成基础取数，了解 GA / 友盟等数据工具。
# 设计与工具
- 熟练使用 Figma、剪映、Photoshop，可独立完成活动物料与短视频剪辑。
# 语言与证书
- 大学英语六级 580 分；普通话二级甲等；全国计算机二级（MS Office）。`
      },
      {
        id: uid(), name: '荣誉证书', icon: 'trophy', size: 9.5, titleSize: 10.5, type: 'text',
        content:
`- 研究生一等学业奖学金（2024 - 2025 学年）
- 校级优秀学生干部（2023 年）
- 全国大学生广告艺术大赛 省级二等奖`
      }
    ]
  };
}

let state = sampleState();

/* ============================================================
 *  3. 渲染：文本模式
 * ============================================================ */
function inlineMd(t) {
  return esc(t)
    .replace(/\*\*(.+?)\*\*/g, '<b>$1</b>')
    .replace(/`(.+?)`/g, '<code>$1</code>');
}

function buildBulletHtml(bs, idx, customChar) {
  if (bs === 'dot') return `<span class="dot"></span>`;
  if (bs === 'circle') return `<span class="dot dot-circle"></span>`;
  if (bs === 'square') return `<span class="dot dot-square"></span>`;
  if (bs === 'dash') return `<span class="bullet-num bullet-custom">—</span>`;
  if (bs === 'number') return `<span class="bullet-num">${idx}.</span>`;
  if (bs === 'number-paren') return `<span class="bullet-num">${idx})</span>`;
  if (bs === 'number-cn') return `<span class="bullet-num">${toCnNum(idx)}、</span>`;
  if (bs === 'letter') return `<span class="bullet-num">${String.fromCharCode(96 + idx)}.</span>`;
  if (bs === 'custom') return `<span class="bullet-num bullet-custom">${esc(customChar || '•')}</span>`;
  return '';
}

function renderTextBody(text) {
  const t = state.theme;
  const bs = t.bulletStyle || 'dot';
  const customChar = t.bulletChar || '•';
  const out = [];
  let listIdx = 0;

  for (const raw of String(text || '').split('\n')) {
    const line = raw.trim();
    if (!line) { out.push('<div class="sp"></div>'); listIdx = 0; continue; }

    const h = line.match(/^(#{1,3})\s+(.*)$/);
    if (h) {
      listIdx = 0;
      const lvl = h[1].length;
      let body = h[2], left = body, right = '';
      if (body.includes('|')) {
        const parts = body.split('|').map(s => s.trim()).filter(Boolean);
        left = parts[0];
        right = parts.slice(1).join(' · ');
      }
      out.push(
        `<div class="row2${lvl > 1 ? ' h2' : ''}">` +
        `<span class="row-l">${inlineMd(left)}</span>` +
        (right ? `<span class="row-r">${inlineMd(right)}</span>` : '') +
        `</div>`
      );
      continue;
    }

    if (/^[-•*·]\s+/.test(line)) {
      listIdx++;
      const content = line.replace(/^[-•*·]\s+/, '');
      if (bs === 'none') {
        out.push(`<div class="p">${inlineMd(content)}</div>`);
      } else {
        out.push(
          `<div class="li">${buildBulletHtml(bs, listIdx, customChar)}` +
          `<span class="li-t">${inlineMd(content)}</span></div>`
        );
      }
      continue;
    }

    listIdx = 0;
    out.push(`<div class="p">${inlineMd(line)}</div>`);
  }
  return out.join('');
}

/* ============================================================
 *  4. 渲染：表格模式
 * ============================================================ */
function renderTableBody(sec) {
  const cols = sec.columns || [];
  if (!cols.length) return '';
  const grid = cols.map(c => `${c.weight || 1}fr`).join(' ');
  const rowsHtml = (sec.rows || []).map(row => {
    return '<div class="tr">' + cols.map((c, i) => {
      const val = row[i] || '';
      const cls = ['tc'];
      if (c.bold) cls.push('bold');
      if (c.muted) cls.push('muted');
      const align = c.align || 'left';
      if (align === 'center') cls.push('center');
      else if (align === 'right') cls.push('right');
      return `<div class="${cls.join(' ')}">${inlineMd(val)}</div>`;
    }).join('') + '</div>';
  }).join('');
  return `<div class="tbl" style="grid-template-columns:${grid}">${rowsHtml}</div>`;
}

function renderSecBody(sec) {
  if (sec.type === 'table') return renderTableBody(sec);
  return renderTextBody(sec.content);
}

/* ============================================================
 *  5. 渲染：预览
 * ============================================================ */
let currentFit = 1;

function getIconColor() {
  const t = state.theme;
  if (t.iconFollow) return t.primary;
  return t.iconColor || t.primary;
}

function renderPreview() {
  const t = state.theme;
  const root = document.documentElement;
  root.style.setProperty('--primary', t.primary);
  root.style.setProperty('--text-color', t.textColor);
  root.style.setProperty('--divider', t.divider);
  root.style.setProperty('--page-pad', t.pagePad + 'mm');

  const flow = $('#pageFlow');
  flow.style.fontFamily = t.font;
  flow.style.setProperty('--lh', t.lineHeight);
  flow.style.setProperty('--name-size', t.nameSize);
  flow.style.setProperty('--contact-size', t.contactSize);
  flow.style.setProperty('--photo-w', t.photoW);
  flow.style.setProperty('--icon-color', getIconColor());
  flow.style.setProperty('--bullet-color', t.bulletColor || t.primary);
  flow.className = 'page-flow ts-' + t.titleStyle + (t.dividerOn ? '' : ' no-divider');
  flow.style.setProperty('--fit', 1);

  const iconColor = getIconColor();

  const contactsHtml = (state.meta.contacts || [])
    .filter(c => (c.text || '').trim())
    .map(c => `<span class="ct">${renderContactIcon(c.icon, iconColor)}<span>${esc(c.text)}</span></span>`)
    .join('');

  const photoHtml = state.meta.photo ? `<img class="photo" src="${state.meta.photo}" alt="证件照">` : '';

  const secHtml = state.sections.map(sec => `
    <section class="sec" style="--sec-size:${sec.size};--sec-title-size:${sec.titleSize};--sec-gap:${t.sectionGap}">
      <h2 class="sec-title">
        ${renderIconHtml(sec.icon, iconColor)}
        <span>${esc(sec.name)}</span>
      </h2>
      <div class="sec-body">${renderSecBody(sec)}</div>
    </section>
  `).join('');

  flow.innerHTML = `
    <header class="rh">
      <div class="rh-left">
        <h1 class="rh-name">${esc(state.meta.name)}</h1>
        ${state.meta.title ? `<div class="rh-title">${esc(state.meta.title)}</div>` : ''}
        ${contactsHtml ? `<div class="rh-contacts">${contactsHtml}</div>` : ''}
      </div>
      ${photoHtml}
    </header>
    <div class="rh-line"></div>
    ${secHtml}
  `;

  autoFit();
}

/* ============================================================
 *  6. 一页自适应
 * ============================================================ */
const MIN_SCALE = 0.30;
const MAX_SCALE = 1.40;

function autoFit() {
  const flow = $('#pageFlow');
  const content = $('#pageContent');
  if (!flow || !content) return;
  const avail = content.clientHeight;
  if (!avail) return;

  const measure = s => {
    flow.style.setProperty('--fit', s);
    return flow.offsetHeight;
  };

  const h1 = measure(1);
  let scale = 1;

  if (h1 > avail) {
    if (measure(MIN_SCALE) > avail) {
      scale = MIN_SCALE;
    } else {
      let lo = MIN_SCALE, hi = 1;
      for (let i = 0; i < 14; i++) {
        const mid = (lo + hi) / 2;
        if (measure(mid) <= avail) lo = mid; else hi = mid;
      }
      scale = lo * 0.997;
    }
  } else if ($('#autoFill').checked) {
    if (measure(MAX_SCALE) <= avail) {
      scale = MAX_SCALE;
    } else {
      let lo = 1, hi = MAX_SCALE;
      for (let i = 0; i < 14; i++) {
        const mid = (lo + hi) / 2;
        if (measure(mid) <= avail) lo = mid; else hi = mid;
      }
      scale = lo * 0.997;
    }
  }

  measure(scale);
  currentFit = scale;
}

/* ============================================================
 *  7. 界面缩放
 * ============================================================ */
let zoomMode = 'auto';
let manualZoom = 1;

function updateZoom() {
  const stage = $('#stage');
  const wrap = $('#paperWrap');
  if (!stage || !wrap) return;

  const PAD = 48;
  const pw = 210 * 96 / 25.4;
  const ph = 297 * 96 / 25.4;

  let z;
  if (zoomMode === 'auto') {
    z = Math.min(
      (stage.clientWidth - PAD) / pw,
      (stage.clientHeight - PAD) / ph,
      1.4
    );
    z = Math.max(0.25, z);
    manualZoom = z;
  } else {
    z = manualZoom;
  }

  wrap.style.setProperty('--zoom', String(z));

  const inp = document.getElementById('zoomInput');
  if (inp && document.activeElement !== inp) {
    inp.value = Math.round(z * 100);
  }
}

function setZoomManual(z) {
  zoomMode = 'manual';
  manualZoom = Math.max(0.20, Math.min(4.0, z));
  updateZoom();
}

/* ============================================================
 *  8. 侧栏渲染
 * ============================================================ */
function renderPhotoBox() {
  const box = $('#photoPreview');
  if (state.meta.photo) {
    box.innerHTML = `
      <img src="${state.meta.photo}" alt="">
      <div class="ph-meta">
        <b>✓ 已保留照片</b>
        照片会随字号一起缩放
      </div>`;
  } else {
    box.innerHTML = `
      <div style="width:56px;height:74px;border-radius:5px;border:1px dashed #dbe3ec;
                  display:flex;align-items:center;justify-content:center;color:#cbd5e1;font-size:20px">👤</div>
      <div class="ph-meta">
        <b>暂无照片</b>
        上传 Word 会自动提取，也可手动上传
      </div>`;
  }
}

function renderContacts() {
  const box = $('#contactList');
  box.innerHTML = '';

  state.meta.contacts.forEach((c, i) => {
    const row = document.createElement('div');
    row.className = 'ct-row';
    const isKey = isIconKey(c.icon);
    const btnInner = isKey
      ? `<svg viewBox="0 0 24 24" aria-hidden="true">${ICON_LIB[c.icon].svg}</svg>`
      : `<span class="emoji">${esc(c.icon || '·')}</span>`;
    row.innerHTML = `
      <button class="ct-icon-btn" data-toggle-icon="${i}" title="点击选择图标">${btnInner}</button>
      <input class="inp ct-t" value="${esc(c.text)}" data-i="${i}" data-k="text" placeholder="内容">
      <button class="ct-del" data-del="${i}" title="删除">✕</button>
    `;
    box.appendChild(row);

    const panel = document.createElement('div');
    panel.className = 'ct-icon-panel';
    panel.dataset.iconPanel = i;
    panel.style.display = 'none';
    panel.innerHTML = `<div class="icon-grid">
      ${Object.entries(ICON_LIB).map(([key, obj]) => `
        <button type="button" data-contact-icon="${i}" data-icon-key="${key}"
                class="${c.icon === key ? 'selected' : ''}${key === 'none' ? ' none-btn' : ''}"
                title="${obj.name}">
          <svg viewBox="0 0 24 24" aria-hidden="true">${obj.svg}</svg>
        </button>
      `).join('')}
    </div>`;
    box.appendChild(panel);
  });
}

function buildIconPicker(currentIcon) {
  const isKey = isIconKey(currentIcon);
  const curInfo = isKey ? ICON_LIB[currentIcon] : null;
  const curName = curInfo ? curInfo.name : (currentIcon || '无');
  const curSvg = curInfo ? `<svg viewBox="0 0 24 24">${curInfo.svg}</svg>` : '';

  const buttons = Object.entries(ICON_LIB).map(([key, obj]) => {
    const selected = currentIcon === key ? ' selected' : '';
    const isNone = key === 'none';
    return `<button type="button" data-pick-icon="${key}" class="${selected}${isNone ? ' none-btn' : ''}" title="${obj.name}">
      <svg viewBox="0 0 24 24" aria-hidden="true">${obj.svg}</svg>
    </button>`;
  }).join('');

  return `
    <div class="icon-picker-label">
      <span>标题图标</span>
      <span class="cur-icon">${curSvg}${esc(curName)}</span>
    </div>
    <div class="icon-grid">${buttons}</div>
  `;
}

function buildTableEditor(sec) {
  const cols = sec.columns || [];
  const rows = sec.rows || [];

  const colRows = cols.map((c, ci) => `
    <div class="tbl-col-row">
      <input value="${esc(c.name)}" data-table-col="${ci}" data-col-field="name" placeholder="列名">
      <select data-table-col="${ci}" data-col-field="align" title="对齐">
        <option value="left"   ${c.align === 'left'   ? 'selected' : ''}>左</option>
        <option value="center" ${c.align === 'center' ? 'selected' : ''}>中</option>
        <option value="right"  ${c.align === 'right'  ? 'selected' : ''}>右</option>
      </select>
      <input type="number" value="${c.weight || 1}" step="0.5" min="0.5"
             data-table-col="${ci}" data-col-field="weight" title="宽度比例">
      <button data-act="del-col" data-idx="${ci}" title="删除此列">✕</button>
    </div>
  `).join('');

  const dataRows = rows.map((row, ri) => `
    <div class="tbl-row">
      ${cols.map((c, ci) => `
        <input value="${esc(row[ci] || '')}" data-table-row="${ri}" data-table-cell="${ci}"
               placeholder="${esc(c.name)}">
      `).join('')}
      <button data-act="del-row" data-idx="${ri}" title="删除此行">✕</button>
    </div>
  `).join('');

  return `
    <div class="tbl-editor">
      <div class="tbl-editor-head">
        <span>列设置（对齐 / 宽度）</span>
        <button data-act="add-col">+ 列</button>
      </div>
      <div class="tbl-cols">${colRows}</div>
      <div class="tbl-editor-head">
        <span>行数据</span>
        <button data-act="add-row">+ 行</button>
      </div>
      <div class="tbl-rows">${dataRows}</div>
    </div>
  `;
}

function renderSections() {
  const wrap = $('#secList');
  const scrollTop = $('.sidebar').scrollTop;
  wrap.innerHTML = '';

  state.sections.forEach((sec, i) => {
    const isTable = sec.type === 'table';
    const card = document.createElement('div');
    card.className = 'sec-card';
    card.dataset.idx = i;

    const editorHtml = isTable
      ? buildTableEditor(sec)
      : `<textarea class="inp" rows="6" data-k="content"
            placeholder="# 主标题 | 2024.09-2027.06&#10;- 要点一&#10;- 要点二">${esc(sec.content || '')}</textarea>`;

    card.innerHTML = `
      <div class="sec-card-head">
        <span class="drag-idx">${i + 1}</span>
        <input class="sec-name" data-k="name" value="${esc(sec.name)}" placeholder="模块名称">
        <div class="sec-actions">
          <button data-act="up" title="上移">↑</button>
          <button data-act="down" title="下移">↓</button>
          <button data-act="del" title="删除">✕</button>
        </div>
      </div>
      <div class="sec-card-body">
        <div class="grid3">
          <label>标题号 <input class="inp" type="number" step="0.5" data-k="titleSize" value="${sec.titleSize}"></label>
          <label>正文号 <input class="inp" type="number" step="0.5" data-k="size" value="${sec.size}"></label>
          <label style="align-self:end;font-size:11px;color:#94a3b8">（图标在下方选）</label>
        </div>
        ${buildIconPicker(sec.icon)}
        <div class="type-switch">
          <button data-type="text"  class="${!isTable ? 'active' : ''}">📝 文本模式</button>
          <button data-type="table" class="${isTable  ? 'active' : ''}">📊 表格模式</button>
        </div>
        ${editorHtml}
      </div>
    `;
    wrap.appendChild(card);
  });

  $('.sidebar').scrollTop = scrollTop;
}

function renderAll() {
  $('#metaName').value = state.meta.name;
  $('#metaTitle').value = state.meta.title;
  renderPhotoBox();
  renderContacts();

  $('#thPrimary').value = state.theme.primary;
  $('#thNameSizeBase').value = state.theme.nameSize;
  $('#thLh').value = state.theme.lineHeight;
  $('#thTitleStyle').value = state.theme.titleStyle;
  $('#thPad').value = state.theme.pagePad;
  $('#thGap').value = state.theme.sectionGap;
  $('#thFont').value = state.theme.font;
  $('#thDivider').checked = state.theme.dividerOn;
  $('#thDividerColor').value = state.theme.divider;
  $('#thBaseSize').value = state.theme.baseSize;
  $('#thPhotoW').value = state.theme.photoW;
  $('#thIconColor').value = state.theme.iconColor || state.theme.primary;
  $('#thIconFollow').checked = !!state.theme.iconFollow;

  $('#thBulletStyle').value = state.theme.bulletStyle || 'dot';
  $('#thBulletChar').value = state.theme.bulletChar || '•';
  $('#thBulletColor').value = state.theme.bulletColor || state.theme.primary;
  $('#thBulletChar').disabled = (state.theme.bulletStyle !== 'custom');

  renderSections();
  renderPreview();
  setTimeout(() => { updateZoom(); }, 0);
}

function initTableData(sec) {
  const lines = String(sec.content || '').split('\n').map(l => l.trim()).filter(Boolean);
  const cols = [
    { name: '学校名称', align: 'left',   weight: 3,   bold: true },
    { name: '专业',     align: 'left',   weight: 3 },
    { name: '平均绩点', align: 'center', weight: 1.5 },
    { name: '时间',     align: 'right',  weight: 2.5, muted: true }
  ];
  const rows = [];
  for (const line of lines) {
    let t = line.replace(/^#\s*/, '').replace(/^[-•*·]\s*/, '');
    if (t.includes('|')) {
      const parts = t.split('|').map(s => s.trim());
      while (parts.length < cols.length) parts.push('');
      rows.push(parts.slice(0, cols.length));
    } else {
      rows.push([t, '', '', '']);
    }
  }
  if (!rows.length) rows.push(cols.map(() => ''));
  sec.columns = cols;
  sec.rows = rows;
}

/* ============================================================
 *  9. 交互绑定
 * ============================================================ */
const refresh = debounce(renderPreview, 120);

$('#metaName').addEventListener('input', e => { state.meta.name = e.target.value; refresh(); });
$('#metaTitle').addEventListener('input', e => { state.meta.title = e.target.value; refresh(); });

$('#contactList').addEventListener('input', e => {
  const i = +e.target.dataset.i, k = e.target.dataset.k;
  if (isNaN(i) || !k) return;
  state.meta.contacts[i][k] = e.target.value;
  refresh();
});

$('#contactList').addEventListener('click', e => {
  const del = e.target.closest('[data-del]');
  if (del) {
    const i = +del.dataset.del;
    state.meta.contacts.splice(i, 1);
    renderContacts(); refresh();
    return;
  }

  const toggle = e.target.closest('[data-toggle-icon]');
  if (toggle) {
    const i = +toggle.dataset.toggleIcon;
    const panel = $(`[data-icon-panel="${i}"]`);
    const isOpen = panel && panel.style.display !== 'none';
    $$('[data-icon-panel]').forEach(p => { p.style.display = 'none'; });
    if (panel && !isOpen) panel.style.display = 'block';
    return;
  }

  const pick = e.target.closest('[data-contact-icon]');
  if (pick) {
    const i = +pick.dataset.contactIcon;
    const key = pick.dataset.iconKey;
    state.meta.contacts[i].icon = (key === 'none') ? '' : key;
    renderContacts();
    refresh();
    return;
  }
});

document.addEventListener('click', e => {
  if (!e.target.closest('.ct-icon-panel') && !e.target.closest('[data-toggle-icon]')) {
    $$('[data-icon-panel]').forEach(p => { p.style.display = 'none'; });
  }
});

$('#btnAddContact').addEventListener('click', () => {
  state.meta.contacts.push({ icon: 'link', text: '' });
  renderContacts(); refresh();
});

$('#btnPhoto').addEventListener('click', () => $('#photoInput').click());
$('#photoInput').addEventListener('change', e => {
  const f = e.target.files[0];
  if (!f) return;
  const fr = new FileReader();
  fr.onload = () => {
    state.meta.photo = fr.result;
    renderPhotoBox();
    renderPreview();
  };
  fr.readAsDataURL(f);
  e.target.value = '';
});
$('#btnPhotoClear').addEventListener('click', () => {
  state.meta.photo = '';
  renderPhotoBox();
  renderPreview();
});

$('#secList').addEventListener('input', e => {
  const card = e.target.closest('.sec-card');
  if (!card) return;
  const idx = +card.dataset.idx;
  const sec = state.sections[idx];
  if (!sec) return;

  const k = e.target.dataset.k;
  if (k) {
    let v = e.target.value;
    if (k === 'size' || k === 'titleSize') v = parseFloat(v) || 9.5;
    sec[k] = v;
    refresh();
    return;
  }

  const tc = e.target.dataset.tableCol;
  if (tc !== undefined) {
    const ci = +tc;
    const field = e.target.dataset.colField;
    let v = e.target.value;
    if (field === 'weight') v = parseFloat(v) || 1;
    if (!sec.columns) sec.columns = [];
    if (!sec.columns[ci]) sec.columns[ci] = {};
    sec.columns[ci][field] = v;
    refresh();
    return;
  }

  const tr = e.target.dataset.tableRow;
  if (tr !== undefined) {
    const ri = +tr;
    const ci = +e.target.dataset.tableCell;
    if (!sec.rows) sec.rows = [];
    if (!sec.rows[ri]) sec.rows[ri] = [];
    sec.rows[ri][ci] = e.target.value;
    refresh();
    return;
  }
});

$('#secList').addEventListener('click', e => {
  const btn = e.target.closest('button');
  if (!btn) return;
  const card = btn.closest('.sec-card');
  const idx = +card.dataset.idx;
  const sec = state.sections[idx];
  if (!sec) return;

  if (btn.dataset.pickIcon !== undefined) {
    const newIcon = btn.dataset.pickIcon;
    sec.icon = (newIcon === 'none') ? '' : newIcon;
    card.querySelectorAll('[data-pick-icon]').forEach(b => {
      b.classList.toggle('selected', b.dataset.pickIcon === newIcon);
    });
    const curInfo = ICON_LIB[newIcon];
    const curBox = card.querySelector('.icon-picker-label .cur-icon');
    if (curBox) {
      curBox.innerHTML = (newIcon === 'none' ? '' : `<svg viewBox="0 0 24 24">${curInfo.svg}</svg>`) + esc(curInfo.name);
    }
    refresh();
    return;
  }

  if (btn.dataset.type) {
    const newType = btn.dataset.type;
    if (newType === 'table' && sec.type !== 'table') {
      if (!sec.columns || !sec.rows) initTableData(sec);
    }
    sec.type = newType;
    renderSections();
    refresh();
    return;
  }

  const act = btn.dataset.act;

  if (act === 'add-col') {
    if (!sec.columns) sec.columns = [];
    sec.columns.push({ name: '新列', align: 'left', weight: 2 });
    if (!sec.rows) sec.rows = [];
    sec.rows.forEach(r => r.push(''));
    renderSections(); refresh();
    return;
  }
  if (act === 'del-col') {
    const ci = +btn.dataset.idx;
    if ((sec.columns || []).length <= 1) { alert('至少保留一列'); return; }
    sec.columns.splice(ci, 1);
    (sec.rows || []).forEach(r => r.splice(ci, 1));
    renderSections(); refresh();
    return;
  }
  if (act === 'add-row') {
    if (!sec.rows) sec.rows = [];
    sec.rows.push((sec.columns || []).map(() => ''));
    renderSections(); refresh();
    return;
  }
  if (act === 'del-row') {
    const ri = +btn.dataset.idx;
    if (!sec.rows || sec.rows.length <= 1) { alert('至少保留一行'); return; }
    sec.rows.splice(ri, 1);
    renderSections(); refresh();
    return;
  }

  if (act === 'up' && idx > 0) {
    [state.sections[idx - 1], state.sections[idx]] = [state.sections[idx], state.sections[idx - 1]];
  } else if (act === 'down' && idx < state.sections.length - 1) {
    [state.sections[idx + 1], state.sections[idx]] = [state.sections[idx], state.sections[idx + 1]];
  } else if (act === 'del') {
    state.sections.splice(idx, 1);
  } else return;

  renderSections();
  refresh();
});

$('#btnAddSection').addEventListener('click', () => {
  state.sections.push({
    id: uid(), name: '新模块', icon: 'pin', type: 'text',
    size: state.theme.baseSize, titleSize: state.theme.baseSize + 1,
    content: '# 条目标题 | 2024.09 - 2027.06\n- 在这里填写你的内容'
  });
  renderSections();
  refresh();
  $('.sidebar').scrollTop = $('.sidebar').scrollHeight;
});

$('#thPrimary').addEventListener('input', e => {
  state.theme.primary = e.target.value;
  if (state.theme.iconFollow) $('#thIconColor').value = e.target.value;
  renderPreview();
});
$('#thNameSizeBase').addEventListener('input', e => { state.theme.nameSize = +e.target.value || 20; renderPreview(); });
$('#thLh').addEventListener('input', e => { state.theme.lineHeight = +e.target.value || 1.45; renderPreview(); });
$('#thTitleStyle').addEventListener('change', e => { state.theme.titleStyle = e.target.value; renderPreview(); });
$('#thPad').addEventListener('input', e => { state.theme.pagePad = +e.target.value || 10; renderPreview(); });
$('#thGap').addEventListener('input', e => { state.theme.sectionGap = +e.target.value || 6; renderPreview(); });
$('#thFont').addEventListener('change', e => { state.theme.font = e.target.value; renderPreview(); });
$('#thDivider').addEventListener('change', e => { state.theme.dividerOn = e.target.checked; renderPreview(); });
$('#thDividerColor').addEventListener('input', e => { state.theme.divider = e.target.value; renderPreview(); });
$('#thPhotoW').addEventListener('input', e => { state.theme.photoW = +e.target.value || 72; renderPreview(); });

$('#thIconColor').addEventListener('input', e => {
  state.theme.iconColor = e.target.value;
  state.theme.iconFollow = false;
  $('#thIconFollow').checked = false;
  renderPreview();
});
$('#thIconFollow').addEventListener('change', e => {
  state.theme.iconFollow = e.target.checked;
  if (e.target.checked) {
    state.theme.iconColor = state.theme.primary;
    $('#thIconColor').value = state.theme.primary;
  }
  renderPreview();
});

$('#thBulletStyle').addEventListener('change', e => {
  state.theme.bulletStyle = e.target.value;
  $('#thBulletChar').disabled = (e.target.value !== 'custom');
  renderPreview();
});
$('#thBulletChar').addEventListener('input', e => {
  state.theme.bulletChar = e.target.value || '•';
  if (state.theme.bulletStyle === 'custom') renderPreview();
});
$('#thBulletColor').addEventListener('input', e => {
  state.theme.bulletColor = e.target.value;
  renderPreview();
});

$('#thBaseSize').addEventListener('input', e => {
  const v = +e.target.value || 9.5;
  state.theme.baseSize = v;
  state.sections.forEach(s => { s.size = v; s.titleSize = +(v + 1).toFixed(1); });
  renderSections();
  renderPreview();
});

$('#autoFill').addEventListener('change', renderPreview);
$('#btnReset').addEventListener('click', () => {
  if (!confirm('确定要恢复为示例简历吗？当前编辑会丢失（建议先保存草稿）。')) return;
  const keepPhoto = state.meta.photo;
  state = sampleState();
  state.meta.photo = keepPhoto;
  renderAll();
});

/* ============================================================
 * 10. 缩放控制 + 面板拖动
 * ============================================================ */
$('#zoomIn').addEventListener('click', () => setZoomManual(manualZoom + 0.1));
$('#zoomOut').addEventListener('click', () => setZoomManual(manualZoom - 0.1));
$('#zoomFit').addEventListener('click', () => {
  zoomMode = 'auto';
  updateZoom();
});
$('#zoomInput').addEventListener('change', e => {
  const v = parseFloat(e.target.value);
  if (!isNaN(v) && v > 0) setZoomManual(v / 100);
});
$('#zoomInput').addEventListener('keydown', e => {
  if (e.key === 'Enter') e.target.blur();
});

(function initWheelZoom() {
  const stage = $('#stage');
  stage.addEventListener('wheel', e => {
    if (e.ctrlKey || e.metaKey) {
      e.preventDefault();
      const delta = e.deltaY > 0 ? -0.05 : 0.05;
      setZoomManual(manualZoom + delta);
    }
  }, { passive: false });
})();

(function initSplitter() {
  const splitter = document.getElementById('splitter');
  const sidebar = document.getElementById('sidebar');
  const app = document.querySelector('.app');
  let dragging = false;

  splitter.addEventListener('mousedown', e => {
    dragging = true;
    splitter.classList.add('dragging');
    document.body.style.cursor = 'col-resize';
    document.body.style.userSelect = 'none';
    e.preventDefault();
  });

  document.addEventListener('mousemove', e => {
    if (!dragging) return;
    const rect = app.getBoundingClientRect();
    let w = e.clientX - rect.left;
    w = Math.max(280, Math.min(640, w));
    sidebar.style.width = w + 'px';
    if (zoomMode === 'auto') updateZoom();
  });

  document.addEventListener('mouseup', () => {
    if (dragging) {
      dragging = false;
      splitter.classList.remove('dragging');
      document.body.style.cursor = '';
      document.body.style.userSelect = '';
    }
  });
})();

/* ============================================================
 * 11. 存档库（隐私保护 + 双存储模式 + 备份）
 * ============================================================ */
const DRAFTS_KEY = 'resume_studio_drafts_v7';
const MAX_DRAFT_BYTES = 4.5 * 1024 * 1024;
const STORAGE_MODE_KEY = 'resume_studio_storage_mode';

const getStorageMode = () => {
  try { return localStorage.getItem(STORAGE_MODE_KEY) || 'local'; }
  catch { return 'local'; }
};
const setStorageMode = mode => {
  try { localStorage.setItem(STORAGE_MODE_KEY, mode); } catch {}
};

const readStorage = storage => {
  try {
    const s = storage === 'session' ? sessionStorage : localStorage;
    return JSON.parse(s.getItem(DRAFTS_KEY) || '[]');
  } catch { return []; }
};

const setDraftsTo = (storage, list) => {
  try {
    const s = storage === 'session' ? sessionStorage : localStorage;
    s.setItem(DRAFTS_KEY, JSON.stringify(list));
    return true;
  } catch (e) {
    alert('存储空间已满，请删除一些旧草稿，或导出备份后清空。');
    return false;
  }
};

const getDrafts = () => {
  const list = [];
  readStorage('local').forEach(d => list.push({ ...d, _storage: 'local' }));
  readStorage('session').forEach(d => list.push({ ...d, _storage: 'session' }));
  list.sort((a, b) => b.updated - a.updated);
  return list;
};

function renderDraftList() {
  const box = $('#draftList');
  const drafts = getDrafts();
  if (!drafts.length) {
    box.innerHTML = '<div style="padding:24px;text-align:center;color:#94a3b8;font-size:13px">还没有保存的草稿<br>在上面输入名字并点击「保存当前」即可</div>';
    return;
  }
  box.innerHTML = drafts.map(d => `
    <div class="draft-item" data-id="${d.id}" data-storage="${d._storage}">
      <div class="d-info">
        <div class="d-name">
          ${esc(d.name)}
          <span class="draft-source ${d._storage}">${d._storage === 'session' ? '🔐 临时' : '💾 本地'}</span>
        </div>
        <div class="d-time">${new Date(d.updated).toLocaleString('zh-CN')}</div>
      </div>
      <div class="d-actions">
        <button class="btn tiny" data-act="load">加载</button>
        <button class="btn tiny" data-act="rename">重命名</button>
        <button class="btn tiny ghost" data-act="del">删除</button>
      </div>
    </div>
  `).join('');
}

$('#btnDrafts').addEventListener('click', () => {
  const mode = getStorageMode();
  document.querySelectorAll('input[name="storageMode"]').forEach(r => {
    r.checked = (r.value === mode);
  });
  renderDraftList();
  $('#draftModal').classList.add('show');
});

document.querySelectorAll('input[name="storageMode"]').forEach(r => {
  r.addEventListener('change', e => setStorageMode(e.target.value));
});

$('#btnSaveDraft').addEventListener('click', () => {
  const nameInput = $('#newDraftName');
  const name = nameInput.value.trim() ||
    ('草稿 · ' + new Date().toLocaleString('zh-CN', { month: '2-digit', day: '2-digit', hour: '2-digit', minute: '2-digit' }));

  const mode = getStorageMode();
  const payload = {
    id: 'd_' + Date.now() + '_' + Math.random().toString(36).slice(2, 6),
    name,
    updated: Date.now(),
    state: JSON.parse(JSON.stringify(state))
  };
  const json = JSON.stringify(payload);
  if (json.length > MAX_DRAFT_BYTES) {
    alert('草稿太大（可能因为照片），请压缩照片后再试。');
    return;
  }
  const list = readStorage(mode);
  list.unshift(payload);
  if (setDraftsTo(mode, list.slice(0, 50))) {
    nameInput.value = '';
    renderDraftList();
  }
});

$('#draftList').addEventListener('click', e => {
  const btn = e.target.closest('button');
  if (!btn) return;
  const item = btn.closest('.draft-item');
  const id = item.dataset.id;
  const storage = item.dataset.storage || 'local';

  const list = readStorage(storage);
  const idx = list.findIndex(d => d.id === id);
  if (idx < 0) return;

  const act = btn.dataset.act;
  if (act === 'load') {
    if (!confirm(`加载草稿「${list[idx].name}」？当前编辑会被覆盖。`)) return;
    state = JSON.parse(JSON.stringify(list[idx].state));
    const th = state.theme;
    if (th.iconColor === undefined) th.iconColor = th.primary;
    if (th.iconFollow === undefined) th.iconFollow = true;
    if (th.bulletStyle === undefined) th.bulletStyle = 'dot';
    if (th.bulletChar === undefined) th.bulletChar = '•';
    if (th.bulletColor === undefined) th.bulletColor = th.primary;
    renderAll();
    $('#draftModal').classList.remove('show');
  } else if (act === 'rename') {
    const nn = prompt('新名称：', list[idx].name);
    if (nn && nn.trim()) {
      list[idx].name = nn.trim();
      list[idx].updated = Date.now();
      setDraftsTo(storage, list);
      renderDraftList();
    }
  } else if (act === 'del') {
    if (!confirm(`删除草稿「${list[idx].name}」？`)) return;
    list.splice(idx, 1);
    setDraftsTo(storage, list);
    renderDraftList();
  }
});

$('#btnExportDrafts').addEventListener('click', () => {
  const all = getDrafts();
  if (!all.length) { alert('还没有草稿可以导出。'); return; }
  const exportData = {
    app: 'Resume Studio',
    version: 1,
    exportedAt: new Date().toISOString(),
    drafts: all.map(d => { const { _storage, ...rest } = d; return rest; })
  };
  const blob = new Blob([JSON.stringify(exportData, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `简历工坊-备份-${new Date().toISOString().slice(0,10)}.json`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  setTimeout(() => URL.revokeObjectURL(url), 1000);
  alert(`已导出 ${all.length} 份草稿。\n换设备时可用「导入备份」恢复。`);
});

$('#btnImportDrafts').addEventListener('click', () => $('#importDraftFile').click());
$('#importDraftFile').addEventListener('change', e => {
  const f = e.target.files[0];
  if (!f) return;
  const fr = new FileReader();
  fr.onload = () => {
    try {
      const data = JSON.parse(fr.result);
      const drafts = Array.isArray(data) ? data : (data.drafts || []);
      if (!drafts.length) { alert('备份文件中没有找到草稿。'); return; }
      const mode = getStorageMode();
      const merged = [...readStorage(mode)];
      let added = 0, updated = 0;
      drafts.forEach(d => {
        if (!d.id || !d.state) return;
        const i = merged.findIndex(x => x.id === d.id);
        if (i >= 0) { merged[i] = d; updated++; }
        else { merged.unshift(d); added++; }
      });
      if (setDraftsTo(mode, merged.slice(0, 100))) {
        renderDraftList();
        alert(`导入完成：新增 ${added} 份，更新 ${updated} 份。`);
      }
    } catch (err) {
      alert('导入失败：文件格式不正确。');
    }
  };
  fr.readAsText(f);
  e.target.value = '';
});

$('#btnClearAllDrafts').addEventListener('click', () => {
  const all = getDrafts();
  if (!all.length) { alert('还没有草稿可以清空。'); return; }
  if (!confirm(`确定要清空全部 ${all.length} 份草稿吗？\n此操作不可恢复，建议先「导出全部备份」。`)) return;
  if (!confirm('再次确认：所有草稿将被永久删除！')) return;
  try { localStorage.removeItem(DRAFTS_KEY); } catch {}
  try { sessionStorage.removeItem(DRAFTS_KEY); } catch {}
  renderDraftList();
  alert('已清空全部草稿。');
});

/* ============================================================
 * 12. 新手引导
 * ============================================================ */
$('#btnGuide').addEventListener('click', () => $('#guideModal').classList.add('show'));

document.addEventListener('click', e => {
  if (e.target.matches('[data-close]') || e.target.classList.contains('modal')) {
    e.target.closest('.modal')?.classList.remove('show');
  }
});

/* ============================================================
 * 13. 导出菜单
 * ============================================================ */
$('#btnExport').addEventListener('click', e => {
  e.stopPropagation();
  $('#exportMenu').classList.toggle('show');
});
document.addEventListener('click', () => $('#exportMenu').classList.remove('show'));
$('#exportMenu').addEventListener('click', e => {
  const fmt = e.target.dataset.fmt;
  if (!fmt) return;
  $('#exportMenu').classList.remove('show');
  if (fmt === 'pdf')  exportPDF();
  if (fmt === 'doc')  exportWord();
  if (fmt === 'html') exportHtml();
});

/* ---------- 13.1 PDF ---------- */
function exportPDF() {
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      window.print();
    });
  });
}

/* ---------- 13.2 Word ---------- */
function iconForWord(iconKey) {
  if (!iconKey) return '';
  if (isIconKey(iconKey)) return ICON_EMOJI_MAP[iconKey] || '';
  return iconKey;
}

function bulletForWord(bs, idx, customChar) {
  switch (bs) {
    case 'dot':          return '●';
    case 'circle':       return '○';
    case 'square':       return '■';
    case 'dash':         return '—';
    case 'number':       return idx + '.';
    case 'number-paren': return idx + ')';
    case 'number-cn':    return toCnNum(idx) + '、';
    case 'letter':       return String.fromCharCode(96 + idx) + '.';
    case 'custom':       return customChar || '•';
    case 'none':         return '';
    default:             return '●';
  }
}

function buildWordBody() {
  const t = state.theme;
  const s = obj => Object.entries(obj).map(([k, v]) => `${k}:${v}`).join(';');
  const parts = [];

  parts.push(`<div style="${s({
    'font-family': t.font.replace(/"/g, "'"),
    'font-size': t.baseSize + 'pt',
    'line-height': t.lineHeight,
    'color': t.textColor
  })}">`);

  parts.push(`<table style="width:100%;border-collapse:collapse;margin-bottom:4pt"><tr><td style="vertical-align:top">`);
  parts.push(`<div style="${s({
    'font-size': t.nameSize + 'pt',
    'font-weight': 'bold',
    'color': t.primary,
    'letter-spacing': '1.2pt'
  })}">${esc(state.meta.name)}</div>`);
  if (state.meta.title) {
    parts.push(`<div style="${s({ 'font-size': (t.contactSize * 1.15) + 'pt', 'color': '#5b6b7d', 'margin-top': '2pt' })}">${esc(state.meta.title)}</div>`);
  }
  const contacts = state.meta.contacts.filter(c => (c.text || '').trim());
  if (contacts.length) {
    parts.push(`<div style="${s({ 'font-size': t.contactSize + 'pt', 'color': '#4a5a6b', 'margin-top': '4pt' })}">`);
    parts.push(contacts.map(c => {
      const ico = iconForWord(c.icon);
      return esc((ico ? ico + ' ' : '') + c.text);
    }).join(' &nbsp;&nbsp;|&nbsp;&nbsp; '));
    parts.push('</div>');
  }
  parts.push(`</td>`);
  if (state.meta.photo) {
    parts.push(`<td style="width:${(t.photoW + 6)}pt;text-align:right;vertical-align:top">
      <img src="${state.meta.photo}" style="width:${t.photoW}pt;height:${Math.round(t.photoW * 4 / 3)}pt;object-fit:cover;border:1px solid #ddd">
    </td>`);
  }
  parts.push(`</tr></table>`);
  parts.push(`<div style="${s({ 'border-top': '1.5pt solid ' + t.primary, 'margin': '4pt 0 6pt 0' })}"></div>`);

  state.sections.forEach((sec, i) => {
    if (i > 0) {
      parts.push(`<div style="${s({
        'border-top': '0.6pt solid ' + (t.dividerOn ? t.divider : '#ffffff'),
        'margin-top': t.sectionGap + 'pt'
      })}"></div>`);
    }
    const ico = iconForWord(sec.icon);
    parts.push(`<h2 style="${s({
      'font-size': sec.titleSize + 'pt',
      'color': t.primary,
      'font-weight': 'bold',
      'margin': '4pt 0 3pt 0',
      'letter-spacing': '0.5pt',
      'font-family': 'inherit'
    })}">${esc((ico ? ico + ' ' : '') + sec.name)}</h2>`);

    if (sec.type === 'table') {
      parts.push(wordTableBody(sec, t));
    } else {
      parts.push(wordTextBody(sec.content, sec.size, t));
    }
  });

  parts.push('</div>');
  return parts.join('');
}

function wordTableBody(sec, t) {
  const cols = sec.columns || [];
  const rows = sec.rows || [];
  if (!cols.length) return '';
  const totalWeight = cols.reduce((s, c) => s + (c.weight || 1), 0);
  const widths = cols.map(c => Math.round((c.weight || 1) / totalWeight * 100));

  let html = `<table style="width:100%;border-collapse:collapse;font-size:${sec.size}pt;line-height:${t.lineHeight}">`;
  for (const row of rows) {
    html += `<tr>`;
    cols.forEach((c, i) => {
      const val = row[i] || '';
      const align = c.align || 'left';
      const bold = c.bold ? 'font-weight:bold;' : '';
      const color = c.muted ? 'color:#6b7a8c;' : 'color:#2f3640;';
      html += `<td style="width:${widths[i]}%;text-align:${align};${bold}${color}padding:1pt 4pt 1pt 0;vertical-align:top">${inlineMd(val)}</td>`;
    });
    html += `</tr>`;
  }
  html += `</table>`;
  return html;
}

function wordTextBody(text, size, t) {
  const out = [];
  const bs = t.bulletStyle || 'dot';
  const bc = t.bulletColor || '#1B3A5C';
  const customChar = t.bulletChar || '•';
  let listIdx = 0;

  for (const raw of String(text || '').split('\n')) {
    const line = raw.trim();
    if (!line) { listIdx = 0; continue; }

    const h = line.match(/^(#{1,3})\s+(.*)$/);
    if (h) {
      listIdx = 0;
      let left = h[2], right = '';
      if (h[2].includes('|')) {
        const p = h[2].split('|').map(s => s.trim()).filter(Boolean);
        left = p[0];
        right = p.slice(1).join(' · ');
      }
      out.push(
        `<table style="width:100%;border-collapse:collapse;margin-bottom:1pt"><tr>` +
        `<td style="font-size:${size}pt;font-weight:bold;color:#2f3640;vertical-align:baseline">${inlineMd(left)}</td>` +
        (right ? `<td style="font-size:${(size * 0.9).toFixed(1)}pt;color:#6b7a8c;text-align:right;white-space:nowrap;vertical-align:baseline">${inlineMd(right)}</td>` : '') +
        `</tr></table>`
      );
      continue;
    }

    if (/^[-•*·]\s+/.test(line)) {
      listIdx++;
      const content = line.replace(/^[-•*·]\s+/, '');
      const bullet = bulletForWord(bs, listIdx, customChar);

      if (bs === 'none' || !bullet) {
        out.push(`<div style="font-size:${size}pt;line-height:${t.lineHeight};margin-bottom:1.2pt;padding-left:1.1em">${inlineMd(content)}</div>`);
      } else {
        out.push(
          `<div style="font-size:${size}pt;line-height:${t.lineHeight};margin-bottom:1.2pt;padding-left:1.4em;text-indent:-1.4em">` +
          `<span style="color:${bc};font-weight:600">${esc(bullet)}</span>&nbsp;${inlineMd(content)}</div>`
        );
      }
      continue;
    }

    listIdx = 0;
    out.push(`<div style="font-size:${size}pt;line-height:${t.lineHeight};margin-bottom:1.2pt">${inlineMd(line)}</div>`);
  }
  return out.join('');
}

function exportWord() {
  const body = buildWordBody();
  const html = `
<html xmlns:o="urn:schemas-microsoft-com:office:office"
      xmlns:w="urn:schemas-microsoft-com:office:word"
      xmlns="http://www.w3.org/TR/REC-html40">
<head>
<meta charset="utf-8">
<title>${esc(state.meta.name)} - 简历</title>
<!--[if gte mso 9]><xml>
<w:WordDocument><w:View>Print</w:View><w:Zoom>100</w:Zoom></w:WordDocument>
</xml><![endif]-->
<style>
@page { size: A4; margin: 10mm 10mm; }
body { margin:0; font-family:"PingFang SC","Microsoft YaHei",sans-serif; }
</style>
</head>
<body>${body}</body>
</html>`;
  downloadBlob(html, (state.meta.name || 'resume') + '.doc', 'application/msword');
}

/* ---------- 13.3 HTML ---------- */
function exportHtml() {
  const t = state.theme;
  const flowEl = document.getElementById('pageFlow');
  const fitVal = flowEl.style.getPropertyValue('--fit') || '1';
  const iconColor = getIconColor();
  const bulletColor = t.bulletColor || t.primary;

  const css = `
    @page { size: A4; margin: 0; }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --primary: ${t.primary};
      --text-color: ${t.textColor};
      --divider: ${t.divider};
      --icon-color: ${iconColor};
      --bullet-color: ${bulletColor};
      --page-pad: ${t.pagePad}mm;
    }
    html, body { background: #eef1f6; font-family: ${t.font}; color: var(--text-color); -webkit-font-smoothing: antialiased; }
    .page { width: 210mm; height: 297mm; padding: var(--page-pad); margin: 24px auto; background: #fff;
            box-shadow: 0 8px 30px rgba(15,23,42,.16); position: relative; overflow: hidden; }
    .page-flow { --fit: ${fitVal}; font-family: ${t.font}; --lh: ${t.lineHeight};
                 --name-size: ${t.nameSize}; --contact-size: ${t.contactSize}; --photo-w: ${t.photoW};
                 --icon-color: ${iconColor}; --bullet-color: ${bulletColor}; }
    .rh { display:flex; align-items:flex-start; gap:12pt; justify-content:space-between; }
    .rh-left { flex:1; min-width:0; }
    .rh-name { font-size:calc(var(--name-size,21) * 1pt * var(--fit)); font-weight:700;
               letter-spacing:1.2pt; color:var(--primary); line-height:1.2; }
    .rh-title { margin-top:calc(2.4 * var(--fit) * 1pt);
                font-size:calc(var(--contact-size,9) * 1.15 * 1pt * var(--fit));
                color:#5b6b7d; letter-spacing:.4pt; }
    .rh-contacts { margin-top:calc(4 * var(--fit) * 1pt); display:flex; flex-wrap:wrap;
                   gap:calc(3 * var(--fit) * 1pt) calc(12 * var(--fit) * 1pt); }
    .ct { font-size:calc(var(--contact-size,9) * 1pt * var(--fit)); color:#4a5a6b;
          display:inline-flex; align-items:center; gap:3pt; white-space:nowrap; }
    .ct-i { display:inline-flex; align-items:center; justify-content:center;
            width:1.05em; height:1.05em; flex:none; color:var(--icon-color, var(--primary)); }
    .ct-i svg { width:100%; height:100%; display:block; stroke:currentColor; fill:none;
                stroke-width:1.9; stroke-linecap:round; stroke-linejoin:round; }
    .ct-i.emoji { width:auto; height:auto; font-size:1.02em; line-height:1; color:inherit; }
    .photo { width:calc(var(--photo-w,72) * 1pt * var(--fit)); aspect-ratio:3/4; height:auto;
             object-fit:cover; border-radius:3pt; flex:none; border:1px solid rgba(0,0,0,.08); display:block; }
    .rh-line { height:calc(1.6 * var(--fit) * 1pt); background:var(--primary); opacity:.9;
               margin:calc(7 * var(--fit) * 1pt) 0 calc(8 * var(--fit) * 1pt); border-radius:2pt; }
    .sec { margin:0; }
    .sec + .sec { border-top:calc(0.6 * var(--fit) * 1pt) solid var(--divider);
                  margin-top:calc(${t.sectionGap} * 1pt * var(--fit)); padding-top:calc(${t.sectionGap} * 1pt * var(--fit)); }
    .no-divider .sec + .sec { border-top:none; padding-top:0; }
    .sec-title { display:flex; align-items:center; gap:calc(4 * var(--fit) * 1pt);
                 margin:0 0 calc(4 * var(--fit) * 1pt); color:var(--primary); font-weight:700;
                 line-height:1.35; font-size:calc(var(--sec-title-size,11) * 1pt * var(--fit)); }
    .sec-ico { display:inline-flex; align-items:center; justify-content:center;
               width:1.15em; height:1.15em; flex:none; color:var(--icon-color, var(--primary)); }
    .sec-ico svg { width:100%; height:100%; display:block; stroke:currentColor; fill:none;
                   stroke-width:1.8; stroke-linecap:round; stroke-linejoin:round; }
    .sec-ico.emoji { font-size:.95em; line-height:1; color:inherit; width:auto; height:auto; }
    .${'ts-' + t.titleStyle} .sec-title { ${t.titleStyle === 'leftbar' ? 'padding-left:calc(6 * var(--fit) * 1pt); border-left:calc(2.4 * var(--fit) * 1pt) solid var(--primary);' : ''}
      ${t.titleStyle === 'underline' ? 'border-bottom:1.1pt solid var(--primary); padding-bottom:calc(1.6 * var(--fit) * 1pt);' : ''}
      ${t.titleStyle === 'filled' ? 'background:var(--primary); color:#fff; padding:calc(2 * var(--fit) * 1pt) calc(7 * var(--fit) * 1pt); border-radius:3pt;' : ''}
      ${t.titleStyle === 'plain' ? 'letter-spacing:.6pt;' : ''} }
    .ts-filled .sec-ico { color: #fff !important; }
    .sec-body { font-size:calc(var(--sec-size,10) * 1pt * var(--fit)); line-height:var(--lh,1.5); color:var(--text-color); }
    .row2 { display:flex; justify-content:space-between; align-items:baseline; gap:8pt; }
    .row2 .row-l { font-weight:600; }
    .row2.h2 .row-l { font-weight:500; }
    .row2 .row-r { color:#6b7a8c; font-size:.9em; white-space:nowrap; flex:none; }
    .p { margin:0 0 calc(1.6 * var(--fit) * 1pt); }
    .li { display:flex; gap:calc(4 * var(--fit) * 1pt); margin-bottom:calc(1.8 * var(--fit) * 1pt); }
    .li-t { flex:1; text-align:justify; }
    .dot { width:calc(3.8 * var(--fit) * 1pt); height:calc(3.8 * var(--fit) * 1pt);
           border-radius:50%; background:var(--bullet-color, var(--primary));
           flex:none; margin-top:.6em; }
    .dot.dot-circle { background:transparent;
                      border:calc(1 * var(--fit) * 1pt) solid var(--bullet-color, var(--primary));
                      width:calc(3.6 * var(--fit) * 1pt); height:calc(3.6 * var(--fit) * 1pt); }
    .dot.dot-square { border-radius:calc(0.5 * var(--fit) * 1pt);
                      width:calc(3.2 * var(--fit) * 1pt); height:calc(3.2 * var(--fit) * 1pt); }
    .bullet-num { flex:none; color:var(--bullet-color, var(--primary)); font-weight:600;
                  font-size:1em; line-height:inherit; min-width:calc(13 * var(--fit) * 1pt);
                  text-align:right; font-variant-numeric:tabular-nums; }
    .bullet-custom { font-weight:700; }
    .sp { height:calc(2.4 * var(--fit) * 1pt); }
    .tbl { display:grid; column-gap:calc(8 * var(--fit) * 1pt); row-gap:calc(1.2 * var(--fit) * 1pt); }
    .tbl .tr { display:contents; }
    .tbl .tc { padding:calc(0.6 * var(--fit) * 1pt) 0;
               font-size:calc(var(--sec-size,10) * 1pt * var(--fit));
               line-height:var(--lh,1.5); color:var(--text-color); word-break:break-word; }
    .tbl .tc.bold { font-weight:600; }
    .tbl .tc.muted { color:#6b7a8c; }
    .tbl .tc.center { text-align:center; }
    .tbl .tc.right { text-align:right; }
    @media print {
      html, body { background:#fff; margin:0; }
      .page { margin:0; box-shadow:none; border-radius:0; width:210mm; height:297mm; }
      * { -webkit-print-color-adjust:exact !important; print-color-adjust:exact !important; }
    }
  `;

  const html = `<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>${esc(state.meta.name)} - 简历</title>
<style>${css}</style>
</head>
<body>
<div class="page">
  <div class="page-flow ${'ts-' + t.titleStyle} ${t.dividerOn ? '' : 'no-divider'}">
    ${flowEl.innerHTML}
  </div>
</div>
</body>
</html>`;

  downloadBlob(html, (state.meta.name || 'resume') + '.html', 'text/html;charset=utf-8');
}

function downloadBlob(content, filename, mime) {
  const blob = new Blob(['\ufeff', content], { type: mime });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  setTimeout(() => URL.revokeObjectURL(url), 1000);
}

/* ============================================================
 * 14. 文件导入
 * ============================================================ */
const SECTION_WORDS = [
  '教育背景','教育经历','学历背景','学历信息','工作经历','实习经历','项目经历',
  '校园经历','实践经历','荣誉','获奖','证书','技能','特长','自我评价','个人总结',
  '求职意向','科研','论文','社团','运营经历','语言能力','个人优势','校园活动',
  '社会工作','培训','兴趣','作品','实习项目','证书荣誉','荣誉证书'
];

const CONTACT_RE = /(@|1[3-9]\d{9}|电话|手机|邮箱|微信|地址|现居|所在地|求职意向|政治面貌)/;
const DATE_RE = /^(\d{4}\s*[.\-/年]|至今|present|now)/i;

const isSectionHeader = t => {
  const s = t.replace(/[\s：:·|｜【】\[\]]/g, '');
  if (!s || s.length > 14) return false;
  return SECTION_WORDS.some(w => s.includes(w));
};
const isDateLine = t => DATE_RE.test(t.trim()) && t.length < 34;

function guessIcon(name) {
  if (/教育|学历|学习/.test(name)) return 'edu';
  if (/实习|工作|职业/.test(name)) return 'work';
  if (/项目/.test(name)) return 'rocket';
  if (/校园|社团|活动/.test(name)) return 'users';
  if (/荣誉|获奖|证书|奖励/.test(name)) return 'trophy';
  if (/技能|特长|能力|工具/.test(name)) return 'wrench';
  if (/自我|评价|总结|优势/.test(name)) return 'bulb';
  if (/科研|论文|学术/.test(name)) return 'book';
  if (/语言|英语/.test(name)) return 'globe';
  if (/兴趣|爱好/.test(name)) return 'palette';
  return 'pin';
}

function guessContactIcon(text) {
  if (/@|邮箱|mail|email/i.test(text)) return 'mail';
  if (/1[3-9]\d{9}|电话|手机|tel|phone/i.test(text)) return 'phone';
  if (/微信|wechat|weixin/i.test(text)) return 'wechat';
  if (/地址|现居|所在地|address/i.test(text)) return 'pin';
  if (/http|https|www\.|链接|blog|github/i.test(text)) return 'link';
  return 'link';
}

function parseBlocks(blocks, keepPhoto) {
  const meta = { name: '你的姓名', title: '', photo: keepPhoto || '', contacts: [] };
  let i = 0;

  const first = (blocks[0] && blocks[0].text || '').trim();
  if (first && first.length <= 6 && !/[@\d]/.test(first) && !/[:：|]/.test(first)) {
    meta.name = first;
    i = 1;
  }

  for (; i < blocks.length && i < 6; i++) {
    const t = blocks[i].text.trim();
    if (!t || isSectionHeader(t)) break;
    if (CONTACT_RE.test(t)) {
      t.split(/[|｜,，;；]+/).map(s => s.trim()).filter(Boolean).forEach(p => {
        if (/求职意向/.test(p)) { meta.title = p.replace(/求职意向[:：]?/, '').trim(); return; }
        meta.contacts.push({ icon: guessContactIcon(p), text: p });
      });
    } else break;
  }

  const sections = [];
  let cur = null;
  const pushLine = (lines, text, isList) => {
    let t = text.trim();
    if (!t) return;
    if (isList) { lines.push('- ' + t.replace(/^[-•*·]\s*/, '')); return; }

    const last = lines[lines.length - 1];
    if (last && last.startsWith('# ') && !last.includes('|') && isDateLine(t)) {
      lines[lines.length - 1] = last + ' | ' + t;
      return;
    }
    if (isDateLine(t)) { lines.push('# ' + t); return; }
    if (t.length <= 40 && !/[。；;]/.test(t)) lines.push('# ' + t);
    else lines.push(t);
  };

  for (; i < blocks.length; i++) {
    const b = blocks[i];
    const t = b.text.trim();
    if (!t) continue;

    if (isSectionHeader(t)) {
      cur = {
        id: uid(),
        name: t.replace(/[\s：:·|｜【】\[\]]/g, '').slice(0, 10),
        icon: guessIcon(t),
        type: 'text',
        size: state.theme.baseSize, titleSize: state.theme.baseSize + 1,
        _lines: []
      };
      sections.push(cur);
    } else {
      if (!cur) {
        cur = { id: uid(), name: '其他经历', icon: 'pin', type: 'text',
                size: state.theme.baseSize, titleSize: state.theme.baseSize + 1, _lines: [] };
        sections.push(cur);
      }
      pushLine(cur._lines, t, b.isList);
    }
  }

  sections.forEach(s => { s.content = s._lines.join('\n').trim(); delete s._lines; });
  return { meta, sections: sections.filter(s => s.content) };
}

async function importDocx(file) {
  if (typeof mammoth === 'undefined') {
    alert('mammoth 库未加载成功，请检查网络后刷新页面。');
    return;
  }
  const buf = await file.arrayBuffer();
  const res = await mammoth.convertToHtml({ arrayBuffer: buf });
  const doc = new DOMParser().parseFromString(res.value, 'text/html');

  const imgs = [...doc.querySelectorAll('img')];
  let photo = state.meta.photo;
  const found = imgs.find(im => im.src && im.src.startsWith('data:'));
  if (found) photo = found.src;

  const blocks = [];
  const walk = node => {
    for (const child of node.children) {
      const tag = child.tagName;
      if (tag === 'TABLE') {
        const rows = child.querySelectorAll('tr');
        rows.forEach(tr => {
          const cells = [...tr.querySelectorAll('td, th')]
            .map(td => (td.textContent || '').replace(/\s+/g, ' ').trim())
            .filter(Boolean);
          if (!cells.length) return;
          if (cells.length === 1) blocks.push({ text: cells[0], isList: false });
          else blocks.push({ text: cells.join(' | '), isList: false });
        });
        continue;
      }
      if (['DIV', 'SECTION', 'ARTICLE', 'BODY'].includes(tag)) { walk(child); continue; }
      if (['P', 'LI', 'H1', 'H2', 'H3', 'H4', 'H5', 'H6'].includes(tag)) {
        const txt = (child.textContent || '').replace(/\s+/g, ' ').trim();
        if (txt) blocks.push({ text: txt, isList: tag === 'LI' });
      }
    }
  };
  walk(doc.body);

  if (!blocks.length) { alert('没有解析到文字内容。'); return; }

  const parsed = parseBlocks(blocks, photo);
  state.meta.name = parsed.meta.name || state.meta.name;
  state.meta.title = parsed.meta.title || state.meta.title;
  state.meta.contacts = parsed.meta.contacts.length ? parsed.meta.contacts : state.meta.contacts;
  state.meta.photo = photo;
  state.sections = parsed.sections.length ? parsed.sections : state.sections;

  renderAll();
  setTimeout(autoFit, 60);
  alert(`解析完成，共识别 ${state.sections.length} 个模块。` + (photo ? '\n✓ 已保留文档中的照片。' : ''));
}

async function importPDF(file) {
  if (!window.pdfjsLib) { alert('PDF 解析库未加载成功。'); return; }
  const buf = await file.arrayBuffer();
  const pdf = await pdfjsLib.getDocument({ data: buf }).promise;
  const blocks = [];

  for (let p = 1; p <= pdf.numPages; p++) {
    const page = await pdf.getPage(p);
    const viewport = page.getViewport({ scale: 1 });
    const tc = await page.getTextContent();

    const items = tc.items
      .filter(it => it.str && it.str.trim())
      .map(it => {
        const tr = it.transform;
        return { str: it.str, x: tr[4], y: tr[5], w: it.width || 0 };
      });
    if (!items.length) continue;

    items.sort((a, b) => b.y - a.y || a.x - b.x);
    const rows = [];
    let cur = null;
    for (const it of items) {
      if (!cur || Math.abs(cur.y - it.y) > 3) {
        cur = { y: it.y, items: [it] };
        rows.push(cur);
      } else cur.items.push(it);
    }

    for (const row of rows) {
      row.items.sort((a, b) => a.x - b.x);
      const segs = [];
      let s = null;
      for (const it of row.items) {
        if (!s) s = { x1: it.x, x2: it.x + it.w, text: it.str };
        else {
          const gap = it.x - s.x2;
          if (gap > viewport.width * 0.08) {
            segs.push(s);
            s = { x1: it.x, x2: it.x + it.w, text: it.str };
          } else {
            s.text += (gap > 2 ? ' ' : '') + it.str;
            s.x2 = it.x + it.w;
          }
        }
      }
      if (s) segs.push(s);

      const segTexts = segs.map(x => x.text.trim()).filter(Boolean);
      if (!segTexts.length) continue;

      let lineText;
      if (segTexts.length === 1) lineText = segTexts[0];
      else if (segTexts.every(t => t.length < 40)) lineText = segTexts.join(' | ');
      else lineText = segTexts.join(' ');
      blocks.push({ text: lineText, isList: false });
    }
  }

  if (!blocks.length) {
    alert('PDF 中没有解析到文字。如果是扫描件，请先 OCR 或改用 Word 文件。');
    return;
  }

  const parsed = parseBlocks(blocks, state.meta.photo);
  state.meta.name = parsed.meta.name || state.meta.name;
  state.meta.title = parsed.meta.title || state.meta.title;
  if (parsed.meta.contacts.length) state.meta.contacts = parsed.meta.contacts;
  if (parsed.sections.length) state.sections = parsed.sections;

  renderAll();
  setTimeout(autoFit, 60);
  alert(`解析完成，共识别 ${state.sections.length} 个模块。`);
}

function importPlainText(text) {
  const blocks = String(text).split('\n')
    .map(l => l.trim())
    .filter(Boolean)
    .map(l => ({ text: l, isList: /^[-•*·]\s+/.test(l) }));
  if (!blocks.length) { alert('请先粘贴简历文本。'); return; }
  const parsed = parseBlocks(blocks, state.meta.photo);
  state.meta.name = parsed.meta.name || state.meta.name;
  state.meta.title = parsed.meta.title || state.meta.title;
  if (parsed.meta.contacts.length) state.meta.contacts = parsed.meta.contacts;
  if (parsed.sections.length) state.sections = parsed.sections;
  renderAll();
  alert(`解析完成，共识别 ${state.sections.length} 个模块。`);
}

$('#btnUpload').addEventListener('click', () => $('#fileInput').click());
$('#fileInput').addEventListener('change', e => {
  const f = e.target.files[0];
  if (!f) return;
  const name = f.name.toLowerCase();
  if (name.endsWith('.pdf')) {
    importPDF(f).catch(err => { console.error(err); alert('PDF 解析失败：' + err.message); });
  } else if (name.endsWith('.docx')) {
    importDocx(f).catch(err => { console.error(err); alert('Word 解析失败：' + err.message); });
  } else {
    alert('仅支持 .docx 和 .pdf 文件。');
  }
  e.target.value = '';
});
$('#btnParse').addEventListener('click', () => importPlainText($('#rawText').value));
$('#btnSample').addEventListener('click', () => {
  if (!confirm('载入示例会覆盖当前编辑，确定继续吗？（建议先保存草稿）')) return;
  const keepPhoto = state.meta.photo;
  state = sampleState();
  state.meta.photo = keepPhoto;
  renderAll();
});

/* ============================================================
 * 15. 窗口大小变化
 * ============================================================ */
window.addEventListener('resize', debounce(() => {
  if (zoomMode === 'auto') updateZoom();
  autoFit();
}, 120));

/* ============================================================
 * 16. 启动
 * ============================================================ */
renderAll();
updateZoom();
setTimeout(() => { updateZoom(); autoFit(); }, 120);
if (document.fonts && document.fonts.ready) {
  document.fonts.ready.then(() => { updateZoom(); autoFit(); });
}
</script>
</body>
</html>
