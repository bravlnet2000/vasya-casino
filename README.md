<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, viewport-fit=cover">
<title>🎰 Казино Вася</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; margin: 0; padding: 0; }
  html, body { height: 100%; overflow: hidden; }
  body {
    display: flex; align-items: center; justify-content: center;
    background: #05020a;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    color: #fff; padding: 10px;
    user-select: none; -webkit-user-select: none;
    touch-action: manipulation;
  }
  body.theme-classic { background: radial-gradient(circle at 50% 0%, #3a0d2e 0%, #1a0620 45%, #05020a 100%); }
  body.theme-neon    { background: radial-gradient(circle at 50% 0%, #0a2540 0%, #050a1a 45%, #000 100%); }
  body.theme-retro   { background: radial-gradient(circle at 50% 0%, #2a1810 0%, #140a05 45%, #000 100%); }
  body.theme-gold    { background: radial-gradient(circle at 50% 0%, #3a2d0a 0%, #1a1305 45%, #000 100%); }

  .screen { position: absolute; inset: 0; display: none; flex-direction: column; align-items: center; justify-content: center; padding: 16px; z-index: 10; }
  .screen.active { display: flex; }

  /* СТАРТ */
  .logo-big { font-size: 60px; line-height: 1; margin-bottom: 8px; animation: floatY 2.5s ease-in-out infinite; filter: drop-shadow(0 0 24px rgba(251,191,36,0.6)); }
  @keyframes floatY { 50% { transform: translateY(-8px); } }
  .logo-title { font-size: 34px; font-weight: 900; letter-spacing: 4px; background: linear-gradient(180deg, #fde047, #fbbf24 45%, #b45309); -webkit-background-clip: text; background-clip: text; color: transparent; text-align: center; margin-bottom: 4px; }
  .logo-sub { font-size: 11px; letter-spacing: 5px; color: #f472b6; margin-bottom: 40px; text-transform: uppercase; }
  .become-btn { padding: 18px 42px; border: none; border-radius: 16px; background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; font-size: 22px; font-weight: 900; letter-spacing: 3px; cursor: pointer; text-transform: uppercase; font-family: inherit; box-shadow: 0 6px 0 #78350f, 0 0 40px rgba(251,191,36,0.5); animation: pulseBtn 1.8s ease-in-out infinite; }
  .become-btn:active { transform: translateY(4px); box-shadow: 0 2px 0 #78350f; }
  @keyframes pulseBtn { 50% { transform: scale(1.04); } }

  /* ВЫБОР */
  .choice-title { font-size: 22px; font-weight: 900; letter-spacing: 2px; color: #fbbf24; margin-bottom: 6px; text-align: center; }
  .choice-sub { font-size: 11px; color: #a78bfa; letter-spacing: 2px; margin-bottom: 26px; }
  .choice-btn { width: 280px; padding: 18px 20px; margin-bottom: 12px; border: 3px solid #b45309; border-radius: 16px; background: linear-gradient(160deg, #2e1a44, #12071e); color: #fff; font-size: 16px; font-weight: 900; letter-spacing: 1px; cursor: pointer; font-family: inherit; text-align: left; display: flex; align-items: center; gap: 14px; box-shadow: 0 0 22px rgba(251,191,36,0.2); }
  .choice-btn:active { transform: scale(0.97); }
  .choice-btn .cb-icon { font-size: 36px; flex-shrink: 0; }
  .choice-btn .cb-text { flex: 1; }
  .choice-btn .cb-name { display: block; color: #fbbf24; font-size: 16px; margin-bottom: 3px; }
  .choice-btn .cb-desc { display: block; color: #a78bfa; font-size: 10px; letter-spacing: 0.5px; font-weight: 600; }
  .back-btn { margin-top: 10px; padding: 10px 22px; border: 2px solid #7c2d12; border-radius: 10px; background: transparent; color: #a78bfa; font-size: 12px; font-weight: 700; font-family: inherit; cursor: pointer; }

  /* ИГРА */
  #screen-game { justify-content: flex-start; padding: 10px; }
  .game-inner { display: flex; flex-direction: column; align-items: center; width: 100%; max-width: 400px; height: 100%; }
  .top-bar { display: flex; align-items: center; gap: 6px; width: 100%; margin-bottom: 8px; }
  .glass { background: #1a0a2e; border: 2px solid #b45309; border-radius: 12px; box-shadow: 0 0 14px rgba(180,83,9,0.3); }
  .balance { flex: 1; display: flex; align-items: center; justify-content: center; gap: 5px; padding: 10px 6px; font-size: 15px; font-weight: 900; color: #fbbf24; }
  .balance.bump { animation: bump 0.3s ease; }
  @keyframes bump { 50% { transform: scale(1.06); } }
  .icon-btn { width: 42px; height: 42px; flex-shrink: 0; font-size: 17px; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #fbbf24; font-family: inherit; }
  .icon-btn:active { transform: scale(0.92); }
  .icon-btn.off { opacity: 0.5; color: #6b7280; border-color: #4b5563; }

  .tabs { display: flex; width: 100%; gap: 3px; margin-bottom: 10px; background: #0a0416; border-radius: 12px; padding: 4px; border: 2px solid #7c2d12; }
  .tab { flex: 1; padding: 9px 2px; border: none; background: transparent; color: #a78bfa; font-weight: 900; font-size: 10px; letter-spacing: 0.3px; border-radius: 8px; cursor: pointer; font-family: inherit; }
  .tab.active { background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; }

  .page { width: 100%; flex: 1; display: none; flex-direction: column; overflow-y: auto; overflow-x: hidden; padding-bottom: 8px; }
  .page.active { display: flex; }
  .page::-webkit-scrollbar { width: 4px; }
  .page::-webkit-scrollbar-thumb { background: #b45309; border-radius: 2px; }

  .title { font-size: 18px; font-weight: 900; letter-spacing: 1.5px; color: #fbbf24; text-align: center; text-shadow: 0 0 10px rgba(251,191,36,0.6); }
  .subtitle { font-size: 9px; letter-spacing: 3px; text-transform: uppercase; color: #f472b6; margin-bottom: 10px; text-align: center; }

  /* СЛОТ */
  .machine { position: relative; width: 100%; padding: 12px; border-radius: 20px; background: linear-gradient(160deg, #2e1a44, #12071e 65%); border: 3px solid #b45309; box-shadow: inset 0 0 30px rgba(251,191,36,0.1), 0 0 24px rgba(251,191,36,0.22); }
  .lights { display: flex; justify-content: center; gap: 6px; margin-bottom: 8px; }
  .lights span { width: 8px; height: 8px; border-radius: 50%; background: #fbbf24; animation: blink 1.5s ease-in-out infinite; }
  .lights span:nth-child(2n) { background: #f472b6; }
  .lights span:nth-child(3n) { background: #38bdf8; }
  @keyframes blink { 50% { opacity: 0.35; } }
  .window { display: flex; gap: 4px; padding: 7px; border-radius: 12px; background: #04020a; border: 2px solid #7c2d12; }
  .reel { flex: 1; height: 74px; overflow: hidden; position: relative; background: #0a0416; border-radius: 7px; }
  .reel::before, .reel::after { content: ''; position: absolute; left: 0; right: 0; height: 12px; z-index: 2; pointer-events: none; }
  .reel::before { top: 0; background: linear-gradient(#0a0416, transparent); }
  .reel::after { bottom: 0; background: linear-gradient(transparent, #0a0416); }
  .strip { position: absolute; top: 0; left: 0; right: 0; will-change: transform; }
  .sym { height: 74px; display: flex; align-items: center; justify-content: center; font-size: 36px; line-height: 1; }

  .bet-row { display: flex; align-items: center; justify-content: space-between; margin-top: 10px; gap: 6px; }
  .bet-label { font-size: 10px; color: #a78bfa; letter-spacing: 0.5px; font-weight: 700; }
  .bet-controls { display: flex; align-items: center; gap: 6px; }
  .bet-btn { width: 36px; height: 36px; border-radius: 50%; background: #1a0a2e; border: 2px solid #b45309; color: #fbbf24; font-size: 19px; font-weight: 900; cursor: pointer; display: flex; align-items: center; justify-content: center; font-family: inherit; }
  .bet-btn:active { transform: scale(0.92); }
  .bet-value { font-size: 17px; font-weight: 900; color: #fff; min-width: 52px; text-align: center; }

  .spin-btn { margin-top: 10px; width: 100%; padding: 15px; border: none; border-radius: 12px; background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; font-size: 16px; font-weight: 900; letter-spacing: 2px; cursor: pointer; text-transform: uppercase; box-shadow: 0 4px 0 #78350f; font-family: inherit; }
  .spin-btn:active:not(:disabled) { transform: translateY(3px); box-shadow: 0 1px 0 #78350f; }
  .spin-btn:disabled { filter: brightness(0.7); cursor: not-allowed; }

  .result { min-height: 42px; margin-top: 10px; width: 100%; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; font-size: 15px; font-weight: 800; line-height: 1.3; padding: 0 4px; }
  .result.win { color: #4ade80; animation: pop 0.4s ease; }
  .result.lose { color: #f87171; animation: pop 0.4s ease; }
  .result.jackpot { color: #fbbf24; animation: pop 0.4s ease; font-size: 16px; }
  @keyframes pop { 0% { transform: scale(0.7); opacity: 0; } 60% { transform: scale(1.06); } 100% { transform: scale(1); } }
  .result .sub { font-size: 12px; opacity: 0.85; font-weight: 600; margin-top: 1px; }

  .machine.win-glow { border-color: #4ade80; box-shadow: 0 0 30px rgba(74,222,128,0.7); }
  .machine.jackpot-glow { border-color: #fbbf24; box-shadow: 0 0 40px rgba(251,191,36,0.9); }
  .machine.lose-glow { border-color: #ef4444; box-shadow: 0 0 25px rgba(239,68,68,0.5); }
  .machine.shake { animation: shakeM 0.4s ease; }
  @keyframes shakeM { 25% { transform: translateX(-6px); } 50% { transform: translateX(6px); } 75% { transform: translateX(-4px); } }

  /* КЕЙС */
  .case-hero { position: relative; width: 100%; padding: 18px; border-radius: 20px; background: linear-gradient(160deg, #2e1a44, #12071e 65%); border: 3px solid #b45309; box-shadow: inset 0 0 30px rgba(251,191,36,0.1), 0 0 24px rgba(251,191,36,0.22); text-align: center; margin-bottom: 10px; }
  .case-image { font-size: 60px; line-height: 1; margin-bottom: 6px; animation: floatY 2.5s ease-in-out infinite; filter: drop-shadow(0 0 18px rgba(251,191,36,0.5)); }
  .case-name { font-size: 17px; font-weight: 900; color: #fbbf24; letter-spacing: 1px; }
  .case-desc { font-size: 11px; color: #a78bfa; margin-top: 4px; margin-bottom: 12px; }
  .case-open-btn { width: 100%; padding: 14px; border: none; border-radius: 12px; background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; font-size: 15px; font-weight: 900; letter-spacing: 2px; cursor: pointer; text-transform: uppercase; box-shadow: 0 4px 0 #78350f; font-family: inherit; }
  .case-open-btn:active:not(:disabled) { transform: translateY(3px); box-shadow: 0 1px 0 #78350f; }
  .case-open-btn:disabled { filter: brightness(0.7); cursor: not-allowed; }

  .case-roulette { display: none; position: relative; width: 100%; height: 110px; overflow: hidden; background: #04020a; border: 3px solid #7c2d12; border-radius: 14px; margin-bottom: 10px; }
  .case-roulette.show { display: block; }
  .case-roulette::before { content: ''; position: absolute; left: 50%; top: 0; bottom: 0; width: 3px; background: linear-gradient(180deg, #fbbf24, #f472b6); transform: translateX(-50%); z-index: 5; box-shadow: 0 0 12px #fbbf24; }
  .case-strip { position: absolute; top: 8px; left: 0; display: flex; gap: 6px; will-change: transform; height: 94px; }
  .case-item { flex-shrink: 0; width: 84px; height: 94px; border-radius: 10px; display: flex; flex-direction: column; align-items: center; justify-content: center; border: 2px solid #374151; background: linear-gradient(180deg, #1a1a2e, #0a0416); position: relative; }
  .case-item .ci-icon { font-size: 32px; line-height: 1; }
  .case-item .ci-name { font-size: 8px; color: #9ca3af; margin-top: 2px; text-align: center; padding: 0 2px; line-height: 1.1; }
  .case-item .ci-stripe { position: absolute; bottom: 0; left: 0; right: 0; height: 3px; border-radius: 0 0 8px 8px; }
  .case-item.r-common { border-color: #6b7280; } .case-item.r-common .ci-stripe { background: #6b7280; }
  .case-item.r-rare { border-color: #3b82f6; } .case-item.r-rare .ci-stripe { background: #3b82f6; }
  .case-item.r-epic { border-color: #a855f7; } .case-item.r-epic .ci-stripe { background: #a855f7; }
  .case-item.r-legend { border-color: #f59e0b; } .case-item.r-legend .ci-stripe { background: #f59e0b; }
  .case-item.r-vasya { border-color: #f472b6; box-shadow: inset 0 0 14px rgba(244,114,182,0.4); } .case-item.r-vasya .ci-stripe { background: linear-gradient(90deg, #f472b6, #fbbf24, #f472b6); }

  /* ИНВЕНТАРЬ */
  .inv-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; padding: 0 4px; }
  .inv-title { font-size: 14px; font-weight: 900; color: #fbbf24; letter-spacing: 1px; }
  .inv-count { font-size: 11px; color: #a78bfa; }
  .inv-total { font-size: 11px; color: #4ade80; font-weight: 900; }
  .inv-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 6px; padding: 2px; }
  .inv-empty { grid-column: 1 / -1; padding: 30px 10px; text-align: center; color: #6b7280; font-size: 12px; }
  .inv-card { position: relative; padding: 8px 4px 6px; border-radius: 10px; display: flex; flex-direction: column; align-items: center; border: 2px solid #374151; background: linear-gradient(180deg, #1a1a2e, #0a0416); cursor: pointer; }
  .inv-card:active { transform: scale(0.95); }
  .inv-card .ic-icon { font-size: 28px; line-height: 1; }
  .inv-card .ic-name { font-size: 9px; color: #d1d5db; margin-top: 3px; text-align: center; line-height: 1.15; min-height: 22px; }
  .inv-card .ic-price { font-size: 10px; color: #4ade80; font-weight: 900; margin-top: 3px; }
  .inv-card .ic-stripe { position: absolute; bottom: 0; left: 0; right: 0; height: 3px; border-radius: 0 0 8px 8px; }
  .inv-card.r-common { border-color: #6b7280; } .inv-card.r-common .ic-stripe { background: #6b7280; }
  .inv-card.r-rare { border-color: #3b82f6; } .inv-card.r-rare .ic-stripe { background: #3b82f6; }
  .inv-card.r-epic { border-color: #a855f7; } .inv-card.r-epic .ic-stripe { background: #a855f7; }
  .inv-card.r-legend { border-color: #f59e0b; box-shadow: 0 0 10px rgba(245,158,11,0.35); } .inv-card.r-legend .ic-stripe { background: #f59e0b; }
  .inv-card.r-vasya { border-color: #f472b6; box-shadow: 0 0 14px rgba(244,114,182,0.5); } .inv-card.r-vasya .ic-stripe { background: linear-gradient(90deg, #f472b6, #fbbf24, #f472b6); }

  .inv-actions { display: flex; gap: 6px; margin-top: 10px; }
  .inv-actions button { flex: 1; padding: 10px 4px; border-radius: 10px; border: 2px solid #b45309; background: #1a0a2e; color: #fbbf24; font-weight: 900; font-size: 11px; font-family: inherit; cursor: pointer; }
  .inv-actions button.primary { background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; border: none; box-shadow: 0 3px 0 #78350f; }
  .inv-actions button:active { transform: translateY(2px); }

  /* АПГРЕЙД */
  .upg-slot { width: 100%; margin-bottom: 8px; background: #0a0416; border: 2px solid #7c2d12; border-radius: 12px; padding: 8px; }
  .upg-slot-label { font-size: 10px; letter-spacing: 1px; color: #a78bfa; font-weight: 900; margin-bottom: 6px; padding-left: 2px; }
  .upg-slot-content { min-height: 88px; display: flex; align-items: center; justify-content: center; border-radius: 8px; cursor: pointer; }
  .upg-slot-content:active { background: rgba(251,191,36,0.1); }
  .upg-slot-content.has-item { flex-direction: column; border: 2px solid #374151; padding: 6px; }
  .upg-slot-content.has-item.r-common { border-color: #6b7280; }
  .upg-slot-content.has-item.r-rare { border-color: #3b82f6; }
  .upg-slot-content.has-item.r-epic { border-color: #a855f7; }
  .upg-slot-content.has-item.r-legend { border-color: #f59e0b; box-shadow: 0 0 14px rgba(245,158,11,0.4); }
  .upg-slot-content.has-item.r-vasya { border-color: #f472b6; box-shadow: 0 0 18px rgba(244,114,182,0.6); }
  .upg-empty { color: #6b7280; font-size: 11px; font-weight: 700; border: 2px dashed #374151; border-radius: 8px; padding: 24px 12px; width: 100%; text-align: center; }
  .upg-item-icon { font-size: 40px; line-height: 1; }
  .upg-item-name { font-size: 12px; font-weight: 900; margin-top: 4px; color: #fff; text-align: center; }
  .upg-item-price { font-size: 11px; color: #4ade80; font-weight: 900; margin-top: 2px; }
  .upg-arrow { font-size: 22px; text-align: center; margin: 4px 0; line-height: 1; color: #fbbf24; }
  .upg-chance-block { margin: 8px 0; padding: 10px; background: #0a0416; border: 2px solid #7c2d12; border-radius: 12px; text-align: center; }
  .upg-chance-label { font-size: 10px; letter-spacing: 1px; color: #a78bfa; font-weight: 900; }
  .upg-chance-value { font-size: 26px; font-weight: 900; color: #fbbf24; text-shadow: 0 0 12px rgba(251,191,36,0.7); margin: 2px 0 6px; }
  .upg-roulette { position: relative; width: 100%; height: 44px; background: #0a0416; border: 2px solid #7c2d12; border-radius: 12px; margin-bottom: 8px; overflow: hidden; }
  .upg-roulette-fill { position: absolute; left: 0; top: 0; bottom: 0; background: linear-gradient(90deg, #4ade80, #fbbf24, #ef4444); opacity: 0.35; width: 50%; }
  .upg-roulette-marker { position: absolute; top: 0; bottom: 0; width: 4px; background: #fff; border-radius: 2px; left: 50%; transform: translateX(-50%); box-shadow: 0 0 12px #fff; z-index: 3; }
  .upg-roulette-target { position: absolute; top: 0; bottom: 0; width: 3px; background: #4ade80; left: 50%; transform: translateX(-50%); z-index: 2; box-shadow: 0 0 10px #4ade80; }

  .upg-picker { position: fixed; inset: 0; z-index: 2200; background: rgba(0,0,0,0.9); display: none; align-items: center; justify-content: center; padding: 16px; }
  .upg-picker.show { display: flex; }
  .upg-picker-box { background: linear-gradient(160deg, #2e1a44, #12071e); border: 3px solid #b45309; border-radius: 18px; padding: 16px; max-width: 360px; width: 100%; max-height: 85vh; overflow: hidden; display: flex; flex-direction: column; }
  .upg-picker-box h3 { color: #fbbf24; margin-bottom: 12px; font-size: 16px; text-align: center; }
  .upg-picker-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 6px; overflow-y: auto; margin-bottom: 12px; padding: 2px; }
  .upg-picker-box button { width: 100%; padding: 12px; border-radius: 10px; background: transparent; border: 2px solid #7c2d12; color: #a78bfa; font-size: 14px; font-weight: 900; font-family: inherit; cursor: pointer; }

  /* МОДАЛКА ПРЕДМЕТА */
  .item-modal { position: fixed; inset: 0; z-index: 2500; background: rgba(0,0,0,0.88); display: none; align-items: center; justify-content: center; padding: 16px; }
  .item-modal.show { display: flex; }
  .item-box { background: linear-gradient(160deg, #2e1a44, #12071e); border: 3px solid #b45309; border-radius: 18px; padding: 20px; max-width: 320px; width: 100%; text-align: center; }
  .item-icon-big { font-size: 60px; line-height: 1; margin-bottom: 6px; }
  .item-name { font-size: 16px; font-weight: 900; margin-bottom: 4px; }
  .item-rarity { font-size: 11px; font-weight: 900; letter-spacing: 1.5px; margin-bottom: 12px; }
  .item-price { font-size: 14px; color: #4ade80; font-weight: 900; margin-bottom: 14px; }
  .item-box button { width: 100%; padding: 12px; border: none; border-radius: 10px; font-size: 14px; font-weight: 900; font-family: inherit; cursor: pointer; margin-bottom: 6px; }
  .item-box .sell { background: linear-gradient(180deg, #4ade80, #16a34a); color: #052e16; }
  .item-box .upgrade-go { background: linear-gradient(180deg, #a855f7, #7c3aed); color: #fff; }
  .item-box .close { background: transparent; border: 2px solid #7c2d12; color: #a78bfa; margin-bottom: 0; }

  .r-common { color: #9ca3af; } .r-rare { color: #60a5fa; } .r-epic { color: #c084fc; } .r-legend { color: #fbbf24; } .r-vasya { color: #f472b6; }

  /* ЛЕНТА */
  .feed-wrap { margin-top: 10px; width: 100%; background: #0a0416; border: 2px solid #7c2d12; border-radius: 12px; overflow: hidden; position: relative; height: 36px; box-shadow: inset 0 0 20px rgba(0,0,0,0.7); flex-shrink: 0; }
  .feed-wrap::before, .feed-wrap::after { content: ''; position: absolute; top: 0; bottom: 0; width: 24px; z-index: 3; pointer-events: none; }
  .feed-wrap::before { left: 28px; background: linear-gradient(90deg, #0a0416, transparent); }
  .feed-wrap::after { right: 0; background: linear-gradient(-90deg, #0a0416, transparent); }
  .feed-label { position: absolute; left: 0; top: 0; bottom: 0; width: 28px; background: #7c2d12; display: flex; align-items: center; justify-content: center; font-size: 13px; z-index: 4; animation: pulseLabel 1.5s ease-in-out infinite; }
  @keyframes pulseLabel { 50% { opacity: 0.7; } }
  .feed-track { position: absolute; left: 28px; top: 0; bottom: 0; display: flex; align-items: center; white-space: nowrap; will-change: transform; }
  .feed-item { display: inline-flex; align-items: center; gap: 4px; padding: 0 12px; font-size: 11px; font-weight: 700; border-right: 1px solid rgba(251,191,36,0.15); height: 100%; }
  .feed-item .f-sym { font-size: 15px; }
  .feed-item .f-name { color: #f472b6; font-weight: 900; }
  .feed-item .f-payout { color: #4ade80; font-weight: 900; }
  .feed-item .f-x { color: #fbbf24; font-weight: 900; }

  .stats { margin-top: 8px; display: flex; gap: 12px; flex-wrap: wrap; justify-content: center; font-size: 11px; color: #a78bfa; }
  .stats b { color: #fff; }

  /* ДОСТИЖЕНИЯ */
  .ach-list { display: flex; flex-direction: column; gap: 6px; }
  .ach-item { display: flex; align-items: center; gap: 10px; padding: 10px 12px; background: #0a0416; border: 2px solid #7c2d12; border-radius: 12px; }
  .ach-item.unlocked { border-color: #4ade80; background: linear-gradient(90deg, rgba(74,222,128,0.1), transparent); }
  .ach-icon { font-size: 26px; flex-shrink: 0; }
  .ach-info { flex: 1; }
  .ach-name { font-size: 13px; font-weight: 900; color: #fff; margin-bottom: 2px; }
  .ach-desc { font-size: 10px; color: #9ca3af; }
  .ach-progress { font-size: 11px; font-weight: 900; color: #fbbf24; }
  .ach-item.unlocked .ach-progress { color: #4ade80; }

  /* СКИНЫ */
  .skin-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; }
  .skin-card { padding: 14px; border-radius: 12px; border: 3px solid #7c2d12; background: #0a0416; cursor: pointer; text-align: center; }
  .skin-card.active { border-color: #4ade80; box-shadow: 0 0 20px rgba(74,222,128,0.4); }
  .skin-card.locked { opacity: 0.5; }
  .skin-preview { width: 100%; height: 40px; border-radius: 8px; margin-bottom: 8px; }
  .skin-name { font-size: 12px; font-weight: 900; color: #fbbf24; }
  .skin-cost { font-size: 11px; color: #4ade80; font-weight: 900; margin-top: 3px; }
  .skin-card.locked .skin-cost { color: #ef4444; }

  /* ЕЖЕДНЕВНЫЙ */
  .daily-box { width: 100%; padding: 20px; background: linear-gradient(160deg, #2e1a44, #12071e); border: 3px solid #b45309; border-radius: 18px; text-align: center; margin-bottom: 12px; box-shadow: 0 0 22px rgba(251,191,36,0.2); }
  .daily-icon { font-size: 60px; line-height: 1; margin-bottom: 8px; animation: floatY 2.5s ease-in-out infinite; }
  .daily-title { font-size: 16px; font-weight: 900; color: #fbbf24; letter-spacing: 1px; margin-bottom: 6px; }
  .daily-desc { font-size: 12px; color: #a78bfa; margin-bottom: 14px; }
  .daily-timer { font-size: 22px; font-weight: 900; color: #fff; letter-spacing: 2px; margin-bottom: 12px; font-variant-numeric: tabular-nums; }

  /* ПОДЕЛИТЬСЯ */
  .share-row { display: flex; gap: 6px; margin-top: 10px; }
  .share-row button { flex: 1; padding: 12px 6px; border-radius: 12px; border: 2px solid #b45309; background: #1a0a2e; color: #fbbf24; font-weight: 900; font-size: 11px; font-family: inherit; cursor: pointer; letter-spacing: 0.3px; }
  .share-row button:active { transform: translateY(2px); }
  .share-row button.tg { background: linear-gradient(180deg, #38bdf8, #0284c7); color: #fff; border-color: #075985; box-shadow: 0 3px 0 #075985; }
  .share-row button.record { background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; border: none; box-shadow: 0 3px 0 #78350f; }

  .confetti { position: fixed; top: -20px; z-index: 999; pointer-events: none; border-radius: 2px; animation: fall linear forwards; }
  @keyframes fall { to { transform: translateY(105vh) rotate(540deg); opacity: 0.5; } }
  .coin { position: fixed; z-index: 998; pointer-events: none; font-size: 20px; animation: coinFly 0.9s ease-out forwards; }
  @keyframes coinFly { 0% { transform: translate(0,0) scale(0.5); opacity: 0; } 20% { opacity: 1; transform: translate(0,-25px) scale(1.05); } 100% { transform: translate(0,-110px) scale(0.6); opacity: 0; } }

  /* МОДАЛКИ */
  .modal { position: fixed; inset: 0; z-index: 2000; background: rgba(0,0,0,0.85); display: none; align-items: center; justify-content: center; padding: 16px; }
  .modal.show { display: flex; }
  .modal-box { background: linear-gradient(160deg, #2e1a44, #12071e); border: 3px solid #b45309; border-radius: 18px; padding: 18px; max-width: 320px; width: 100%; text-align: center; }
  .modal-box h3 { color: #fbbf24; margin-bottom: 10px; font-size: 17px; }
  .modal-box p { color: #cbd5e1; font-size: 13px; margin-bottom: 12px; line-height: 1.5; }
  .modal-box button, .modal-box input { width: 100%; padding: 12px; border: none; border-radius: 10px; background: linear-gradient(180deg, #fde047, #f59e0b 55%, #b45309); color: #3b1a00; font-size: 15px; font-weight: 900; cursor: pointer; margin-bottom: 6px; font-family: inherit; }
  .modal-box input { background: #0f0518; color: #fff; border: 2px solid #b45309; text-align: center; font-weight: 600; }
  .modal-box input::placeholder { color: #6b7280; }
  .modal-box button.ghost { background: transparent; border: 2px solid #7c2d12; color: #a78bfa; margin-bottom: 0; }

  #err { position: fixed; bottom: 6px; left: 6px; right: 6px; background: #7f1d1d; color: #fff; font-size: 11px; padding: 6px; border-radius: 6px; z-index: 9999; display: none; word-break: break-word; }
</style>
</head>
<body class="theme-classic">

<!-- СТАРТ -->
<div class="screen active" id="screen-start">
  <div class="logo-big">🎩</div>
  <div class="logo-title">КАЗИНО</div>
  <div class="logo-sub">имени Васи</div>
  <button type="button" class="become-btn" id="becomeBtn">СТАТЬ ВАСЕЙ</button>
</div>

<!-- ВЫБОР -->
<div class="screen" id="screen-choice">
  <div class="choice-title">Выбери путь, Вася</div>
  <div class="choice-sub">Куда направимся?</div>

  <button type="button" class="choice-btn" id="goCasino">
    <span class="cb-icon">🎰</span>
    <span class="cb-text">
      <span class="cb-name">КАЗИНО</span>
      <span class="cb-desc">Крути барабаны, лови Васю</span>
    </span>
  </button>

  <button type="button" class="choice-btn" id="goCase">
    <span class="cb-icon">🎁</span>
    <span class="cb-text">
      <span class="cb-name">КЕЙС ЮР ЮРЫЧА</span>
      <span class="cb-desc">Открывай кейсы, собирай скины</span>
    </span>
  </button>

  <button type="button" class="back-btn" id="backToStart">← Назад</button>
</div>

<!-- ИГРА -->
<div class="screen" id="screen-game">
  <div class="game-inner">
    <div class="top-bar">
      <button type="button" class="icon-btn glass" id="sndBtn">🔇</button>
      <div class="balance glass" id="balEl">💰 <span id="balVal">1000</span></div>
      <button type="button" class="icon-btn glass" id="menuBtn">☰</button>
    </div>

    <div class="tabs">
      <button type="button" class="tab active" data-page="slot">🎰 СЛОТ</button>
      <button type="button" class="tab" data-page="case">🎁 КЕЙС</button>
      <button type="button" class="tab" data-page="inv">🎒 ИНВ</button>
      <button type="button" class="tab" data-page="upg">⬆️ АП</button>
      <button type="button" class="tab" data-page="more">📦 ЕЩЁ</button>
    </div>

    <!-- СЛОТ -->
    <div class="page active" id="page-slot">
      <div class="title">🎰 КАЗИНО ВАСЯ 🎰</div>
      <div class="subtitle">Deluxe</div>
      <div class="machine" id="machine">
        <div class="lights"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></div>
        <div class="window">
          <div class="reel"><div class="strip"><div class="sym">🍒</div></div></div>
          <div class="reel"><div class="strip"><div class="sym">🍒</div></div></div>
          <div class="reel"><div class="strip"><div class="sym">🍒</div></div></div>
        </div>
        <div class="bet-row">
          <span class="bet-label">СТАВКА</span>
          <div class="bet-controls">
            <button type="button" class="bet-btn" id="bMinus">−</button>
            <div class="bet-value" id="betVal">10</div>
            <button type="button" class="bet-btn" id="bPlus">+</button>
          </div>
          <span class="bet-label">×2000</span>
        </div>
        <button type="button" class="spin-btn" id="spinBtn">Крутить</button>
      </div>
      <div class="result" id="result"></div>
      <div class="stats">
        <span>Побед: <b id="winsEl">0</b></span>
        <span>Попыток: <b id="spinsEl">0</b></span>
        <span>Джекпотов: <b id="jpEl">0</b></span>
      </div>
    </div>

    <!-- КЕЙС -->
    <div class="page" id="page-case">
      <div class="title">🎁 КЕЙС ЮР ЮРЫЧА</div>
      <div class="subtitle">Открой, попробуй удачу</div>
      <div class="case-hero">
        <div class="case-image">🎁</div>
        <div class="case-name">Кейс Юр Юрыча</div>
        <div class="case-desc">100 монет • 18 предметов • 5 редкостей</div>
        <button type="button" class="case-open-btn" id="caseOpenBtn">🔓 Открыть — 100 💰</button>
      </div>
      <div class="case-roulette" id="caseRoulette">
        <div class="case-strip" id="caseStrip"></div>
      </div>
      <div class="result" id="caseResult"></div>
      <div class="inv-actions" style="margin-top: 12px;">
        <button type="button" id="caseOpen5Btn">🔓×5 — 450 💰</button>
        <button type="button" id="caseSellDupesBtn">💸 Дубли</button>
      </div>
    </div>

    <!-- ИНВЕНТАРЬ -->
    <div class="page" id="page-inv">
      <div class="title">🎒 ИНВЕНТАРЬ</div>
      <div class="subtitle">Нажми на предмет</div>
      <div class="inv-header">
        <div class="inv-title">ПРЕДМЕТЫ</div>
        <div>
          <div class="inv-count" id="invCount">0 шт.</div>
          <div class="inv-total" id="invTotal">0 💰</div>
        </div>
      </div>
      <div class="inv-grid" id="invGrid"></div>
      <div class="inv-actions">
        <button type="button" id="invSellAllBtn">💸 Продать всё</button>
        <button type="button" class="primary" id="invCloseBtn">🎰 В казино</button>
      </div>
    </div>

    <!-- АПГРЕЙД -->
    <div class="page" id="page-upg">
      <div class="title">⬆️ АПГРЕЙД</div>
      <div class="subtitle">Обменяй на что-то лучше</div>

      <div class="upg-slot">
        <div class="upg-slot-label">ТВОЙ ПРЕДМЕТ</div>
        <div class="upg-slot-content" id="upgMyContent">
          <div class="upg-empty">Нажми, чтобы выбрать</div>
        </div>
      </div>

      <div class="upg-arrow">⬇️</div>

      <div class="upg-slot">
        <div class="upg-slot-label">ХОЧУ ПОЛУЧИТЬ</div>
        <div class="upg-slot-content" id="upgTargetContent">
          <div class="upg-empty">Нажми, чтобы выбрать</div>
        </div>
      </div>

      <div class="upg-chance-block">
        <div class="upg-chance-label">ШАНС УСПЕХА</div>
        <div class="upg-chance-value" id="upgChanceVal">—</div>
      </div>

      <div class="upg-roulette">
        <div class="upg-roulette-fill" id="upgRouletteFill"></div>
        <div class="upg-roulette-marker" id="upgRouletteMarker"></div>
        <div class="upg-roulette-target" id="upgRouletteTarget"></div>
      </div>

      <div class="result" id="upgResult"></div>
      <button type="button" class="spin-btn" id="upgBtn" disabled>⬆️ Апгрейд</button>
    </div>

    <!-- ЕЩЁ -->
    <div class="page" id="page-more">
      <div class="title">📦 ЕЩЁ</div>
      <div class="subtitle">Бонусы, скины, награды</div>

      <div class="daily-box">
        <div class="daily-icon">🎁</div>
        <div class="daily-title">ЕЖЕДНЕВНЫЙ БОНУС</div>
        <div class="daily-desc" id="dailyDesc">Забирай 500 монет каждые 24 часа</div>
        <div class="daily-timer" id="dailyTimer">Готово!</div>
        <button type="button" class="case-open-btn" id="dailyBtn">🎁 ЗАБРАТЬ 500</button>
      </div>

      <div class="inv-header" style="margin-top: 6px;">
        <div class="inv-title">🎨 СКИНЫ</div>
      </div>
      <div class="skin-grid" id="skinGrid"></div>

      <div class="inv-header" style="margin-top: 12px;">
        <div class="inv-title">🏅 ДОСТИЖЕНИЯ</div>
        <div class="inv-count" id="achCount">0 / 8</div>
      </div>
      <div class="ach-list" id="achList"></div>

      <div class="inv-header" style="margin-top: 14px;">
        <div class="inv-title">👥 ПОЗВАТЬ ДРУЗЕЙ</div>
      </div>
      <p style="font-size: 11px; color: #a78bfa; text-align: center; margin-bottom: 6px; padding: 0 10px;">
        Скинь ссылку друзьям в Telegram — играйте вместе!
      </p>
      <div class="share-row">
        <button type="button" class="tg" id="inviteBtn">📨 ПРИГЛАСИТЬ</button>
        <button type="button" class="record" id="shareRecordBtn">🏆 РЕКОРД</button>
      </div>
    </div>

    <!-- ЛЕНТА -->
    <div class="feed-wrap">
      <div class="feed-label">🔴</div>
      <div class="feed-track" id="feedTrack"></div>
    </div>
  </div>
</div>

<!-- ПИКЕР АПГРЕЙДА -->
<div class="upg-picker" id="upgPicker">
  <div class="upg-picker-box">
    <h3 id="upgPickerTitle">Выбери предмет</h3>
    <div class="upg-picker-grid" id="upgPickerGrid"></div>
    <button type="button" id="upgPickerClose">Отмена</button>
  </div>
</div>

<!-- МОДАЛКА ПРЕДМЕТА -->
<div class="item-modal" id="itemModal">
  <div class="item-box">
    <div class="item-icon-big" id="imIcon">🍒</div>
    <div class="item-name" id="imName">Яблоко</div>
    <div class="item-rarity" id="imRarity">ОБЫЧНЫЙ</div>
    <div class="item-price" id="imPrice">+10 💰</div>
    <button type="button" class="sell" id="imSellBtn">💰 Продать</button>
    <button type="button" class="upgrade-go" id="imUpgradeBtn">⬆️ В апгрейд</button>
    <button type="button" class="close" id="imCloseBtn">Закрыть</button>
  </div>
</div>

<!-- МОДАЛКА БОНУСА -->
<div class="modal" id="modal">
  <div class="modal-box">
    <h3>💸 Монеты кончились</h3>
    <p>Бонус от Васи — 500 монет!</p>
    <button type="button" id="bonusBtn">💰 Получить 500</button>
    <button type="button" class="ghost" id="resetBtn2">🔄 Сброс</button>
  </div>
</div>

<!-- МОДАЛКА ИМЕНИ -->
<div class="modal" id="nameModal">
  <div class="modal-box">
    <h3>👤 Твоё имя</h3>
    <p>Оно будет показываться в живой ленте</p>
    <input type="text" id="nameInput" placeholder="Имя" maxlength="16" autocomplete="off">
    <button type="button" id="nameSave">Сохранить</button>
    <button type="button" class="ghost" id="nameCancel">Отмена</button>
  </div>
</div>

<!-- МОДАЛКА МЕНЮ -->
<div class="modal" id="menuModal">
  <div class="modal-box">
    <h3>☰ Меню</h3>
    <button type="button" id="menuChangeName">👤 Сменить имя</button>
    <button type="button" id="menuInvite">📨 Пригласить друзей</button>
    <button type="button" id="menuChangeMode">🚪 Сменить режим</button>
    <button type="button" class="ghost" id="menuClose">Закрыть</button>
  </div>
</div>

<!-- МОДАЛКА СКИНА -->
<div class="modal" id="skinModal">
  <div class="modal-box">
    <h3 id="skinModalTitle">🎨 Скин</h3>
    <p id="skinModalDesc"></p>
    <button type="button" id="skinBuyBtn">Купить</button>
    <button type="button" class="ghost" id="skinCancelBtn">Отмена</button>
  </div>
</div>

<div id="err"></div>

<script>
(function() {
  'use strict';

  /* ============================================
     TELEGRAM WEBAPP
  ============================================ */
  var tg = (window.Telegram && window.Telegram.WebApp) ? window.Telegram.WebApp : null;
  var tgUser = null;
  if (tg) {
    try {
      tg.ready();
      tg.expand();
      if (tg.setHeaderColor) tg.setHeaderColor('#05020a');
      if (tg.setBackgroundColor) tg.setBackgroundColor('#05020a');
      if (tg.disableVerticalSwipes) tg.disableVerticalSwipes();
      if (tg.initDataUnsafe && tg.initDataUnsafe.user) tgUser = tg.initDataUnsafe.user;
    } catch(e) {}
  }

  function haptic(kind) {
    if (!tg || !tg.HapticFeedback) return;
    try {
      if (kind === 'light') tg.HapticFeedback.impactOccurred('light');
      else if (kind === 'medium') tg.HapticFeedback.impactOccurred('medium');
      else if (kind === 'heavy') tg.HapticFeedback.impactOccurred('heavy');
      else if (kind === 'success') tg.HapticFeedback.notificationOccurred('success');
      else if (kind === 'error') tg.HapticFeedback.notificationOccurred('error');
      else if (kind === 'warning') tg.HapticFeedback.notificationOccurred('warning');
    } catch(e) {}
  }

  var errBox = document.getElementById('err');
  window.addEventListener('error', function(e) {
    errBox.style.display = 'block';
    errBox.textContent = 'Ошибка: ' + e.message;
  });

  /* ============================================
     КОНСТАНТЫ
  ============================================ */
  var BETS = [10, 25, 50, 100, 250, 500, 1000, 2500, 5000];
  var S_KEY = 'vc9';
  var CASE_PRICE = 100;
  var CASE_PRICE_5 = 450;
  var DAILY_AMOUNT = 500;
  var DAILY_CD = 24 * 60 * 60 * 1000;

  var SYMBOLS = ['🍒','🍋','🔔','💎','7️⃣','👨'];
  var WEIGHTS = [22, 22, 18, 13, 11, 8];
  var VASYA = '👨', GOLD = '🥇', LEGEND = '👑';
  var SYM_H = 74;
  var PAYOUTS = { '🍒': 5, '🍋': 8, '🔔': 15, '💎': 30, '7️⃣': 50, '👨': 200 };
  var PAY_GOLD = 500, PAY_LEGEND = 2000;

  var RARITY = {
    common: { name: 'ОБЫЧНЫЙ', cls: 'r-common' },
    rare:   { name: 'РЕДКИЙ', cls: 'r-rare' },
    epic:   { name: 'ЭПИЧЕСКИЙ', cls: 'r-epic' },
    legend: { name: 'ЛЕГЕНДА', cls: 'r-legend' },
    vasya:  { name: 'ВАСЯ', cls: 'r-vasya' }
  };

  var ITEMS = [
    { id: 'c1', icon: '🥒', name: 'Огурец',           rarity: 'common', price: 15 },
    { id: 'c2', icon: '🍞', name: 'Хлебушек',         rarity: 'common', price: 20 },
    { id: 'c3', icon: '🧦', name: 'Носок',            rarity: 'common', price: 12 },
    { id: 'c4', icon: '🥔', name: 'Картошка',         rarity: 'common', price: 18 },
    { id: 'c5', icon: '🧄', name: 'Чеснок',           rarity: 'common', price: 22 },
    { id: 'c6', icon: '🍺', name: 'Пивасик',          rarity: 'common', price: 25 },
    { id: 'r1', icon: '🎩', name: 'Шляпа Васи',       rarity: 'rare',   price: 80 },
    { id: 'r2', icon: '🥊', name: 'Перчатка',         rarity: 'rare',   price: 90 },
    { id: 'r3', icon: '🕶', name: 'Очки Юр Юрыча',    rarity: 'rare',   price: 100 },
    { id: 'r4', icon: '🎸', name: 'Гитара',           rarity: 'rare',   price: 110 },
    { id: 'e1', icon: '💎', name: 'Алмаз',            rarity: 'epic',   price: 350 },
    { id: 'e2', icon: '🔥', name: 'Огненный нож',     rarity: 'epic',   price: 400 },
    { id: 'e3', icon: '⚡', name: 'Молния',           rarity: 'epic',   price: 450 },
    { id: 'l1', icon: '👑', name: 'Корона Васи',      rarity: 'legend', price: 1500 },
    { id: 'l2', icon: '🏆', name: 'Кубок Юр Юрыча',   rarity: 'legend', price: 1800 },
    { id: 'v1', icon: '👨', name: 'Вася Обычный',     rarity: 'vasya',  price: 5000 },
    { id: 'v2', icon: '🥇', name: 'Вася Золотой',     rarity: 'vasya',  price: 12000 },
    { id: 'v3', icon: '👑', name: 'Вася Легендарный', rarity: 'vasya',  price: 50000 }
  ];

  var SKINS = [
    { id: 'classic', name: 'Классика',  cost: 0,    theme: 'theme-classic', preview: 'radial-gradient(circle at 50% 0%, #3a0d2e, #05020a)' },
    { id: 'neon',    name: 'Неон',      cost: 2000, theme: 'theme-neon',    preview: 'radial-gradient(circle at 50% 0%, #0a2540, #000)' },
    { id: 'retro',   name: 'Ретро',     cost: 5000, theme: 'theme-retro',   preview: 'radial-gradient(circle at 50% 0%, #2a1810, #000)' },
    { id: 'gold',    name: 'Золото',    cost: 15000,theme: 'theme-gold',    preview: 'radial-gradient(circle at 50% 0%, #3a2d0a, #000)' }
  ];

  var ACHIEVEMENTS = [
    { id: 'first_win',  icon: '🎉', name: 'Первая победа',  desc: 'Выиграй в слоте',     check: function(s) { return s.wins >= 1; },          progress: function(s) { return Math.min(1, s.wins) + '/1'; } },
    { id: 'ten_wins',   icon: '🔥', name: '10 побед',       desc: 'Выиграй 10 раз',      check: function(s) { return s.wins >= 10; },         progress: function(s) { return Math.min(10, s.wins) + '/10'; } },
    { id: 'hundred',    icon: '💯', name: '100 побед',      desc: 'Выиграй 100 раз',     check: function(s) { return s.wins >= 100; },        progress: function(s) { return Math.min(100, s.wins) + '/100'; } },
    { id: 'first_jack', icon: '👨', name: 'Первый Вася',    desc: 'Поймай джекпот',      check: function(s) { return s.jackpots >= 1; },      progress: function(s) { return Math.min(1, s.jackpots) + '/1'; } },
    { id: 'five_jack',  icon: '👑', name: '5 джекпотов',    desc: 'Поймай 5 джекпотов',  check: function(s) { return s.jackpots >= 5; },      progress: function(s) { return Math.min(5, s.jackpots) + '/5'; } },
    { id: 'case_10',    icon: '🎁', name: '10 кейсов',      desc: 'Открой 10 кейсов',    check: function(s) { return s.casesOpened >= 10; },  progress: function(s) { return Math.min(10, s.casesOpened) + '/10'; } },
    { id: 'vasya_item', icon: '💗', name: 'Настоящий Вася', desc: 'Выбей Васю из кейса', check: function(s) { return s.gotVasya; },           progress: function(s) { return s.gotVasya ? '1/1' : '0/1'; } },
    { id: 'rich',       icon: '💰', name: 'Богач',          desc: 'Накопи 50000 монет',  check: function(s) { return s.balance >= 50000; },   progress: function(s) { return Math.min(50000, s.balance) + '/50000'; } }
  ];

  /* ============================================
     СОСТОЯНИЕ
  ============================================ */
  var DEF = {
    balance: 1000, bet: 10, wins: 0, spins: 0, jackpots: 0, maxWin: 0,
    casesOpened: 0, gotVasya: false, sound: false, name: '',
    inventory: [], unlockedAch: [], unlockedSkins: ['classic'],
    activeSkin: 'classic', lastDaily: 0
  };
  var S = {};

  function load() {
    for (var k in DEF) S[k] = DEF[k];
    try {
      var raw = localStorage.getItem(S_KEY);
      if (raw) {
        var o = JSON.parse(raw);
        if (o && typeof o.balance === 'number' && o.balance >= 0) {
          for (var k2 in DEF) if (k2 in o) S[k2] = o[k2];
          if (!Array.isArray(S.inventory)) S.inventory = [];
          if (!Array.isArray(S.unlockedAch)) S.unlockedAch = [];
          if (!Array.isArray(S.unlockedSkins)) S.unlockedSkins = ['classic'];
        }
      }
    } catch(e) { S.inventory = []; S.unlockedAch = []; S.unlockedSkins = ['classic']; }
    applySkin();
  }

  var saveTimer = null;
  function save() {
    if (saveTimer) return;
    saveTimer = setTimeout(function() {
      saveTimer = null;
      try { localStorage.setItem(S_KEY, JSON.stringify(S)); } catch(e){}
    }, 200);
  }
  load();

  // имя из Telegram
  if (!S.name && tgUser) {
    S.name = tgUser.first_name || tgUser.username || 'Игрок';
    save();
  }

  function applySkin() {
    var sk = null;
    for (var i = 0; i < SKINS.length; i++) if (SKINS[i].id === S.activeSkin) sk = SKINS[i];
    document.body.className = (sk || SKINS[0]).theme;
  }

  /* ============================================
     DOM
  ============================================ */
  var $ = function(id) { return document.getElementById(id); };
  var screens = { start: $('screen-start'), choice: $('screen-choice'), game: $('screen-game') };
  function showScreen(name) {
    for (var k in screens) screens[k].classList.toggle('active', k === name);
  }

  var machine = $('machine'), resultEl = $('result'), spinBtn = $('spinBtn'),
      balEl = $('balEl'), balVal = $('balVal'), betVal = $('betVal'),
      winsEl = $('winsEl'), spinsEl = $('spinsEl'), jpEl = $('jpEl'),
      sndBtn = $('sndBtn'), modal = $('modal'),
      nameModal = $('nameModal'), nameInput = $('nameInput'),
      menuModal = $('menuModal'),
      feedTrack = $('feedTrack'),
      caseRoulette = $('caseRoulette'), caseStrip = $('caseStrip'),
      caseOpenBtn = $('caseOpenBtn'), caseResult = $('caseResult'),
      invGrid = $('invGrid'), invCount = $('invCount'), invTotal = $('invTotal'),
      itemModal = $('itemModal');

  var reels = document.querySelectorAll('.reel');
  var stripEls = [reels[0].querySelector('.strip'), reels[1].querySelector('.strip'), reels[2].querySelector('.strip')];

  var spinning = false, caseSpinning = false, currentItem = null;

  /* ============================================
     ЗВУК
  ============================================ */
  var ac = null, musicOn = false, musicStep = 0, musicNext = 0;

  function ensureAC() {
    if (!ac) {
      try {
        var AC = window.AudioContext || window.webkitAudioContext;
        if (AC) ac = new AC();
      } catch(e) { ac = null; }
    }
    if (ac && ac.state === 'suspended') ac.resume().catch(function(){});
    return ac;
  }

  function note(freq, t, dur, type, vol) {
    if (!ac) return;
    try {
      var o = ac.createOscillator(), g = ac.createGain();
      o.type = type; o.frequency.value = freq;
      g.gain.setValueAtTime(0.0001, t);
      g.gain.exponentialRampToValueAtTime(vol, t + 0.01);
      g.gain.exponentialRampToValueAtTime(0.0001, t + dur);
      o.connect(g); g.connect(ac.destination);
      o.start(t); o.stop(t + dur + 0.02);
    } catch(e){}
  }

  function sfx(freq, dur, type, vol, delay) {
    if (!S.sound) return;
    var c = ensureAC(); if (!c) return;
    note(freq, c.currentTime + (delay || 0), dur, type || 'sine', vol || 0.1);
  }

  function sTick() { sfx(760, 0.05, 'square', 0.06); }
  function sWin()  { [523,659,784,1047].forEach(function(f,i){ sfx(f, 0.28, 'triangle', 0.11, i*0.08); }); }
  function sJack() { [523,659,784,1047,1319].forEach(function(f,i){ sfx(f, 0.3, 'triangle', 0.13, i*0.06); }); }
  function sLose() { [330,262,196].forEach(function(f,i){ sfx(f, 0.22, 'sawtooth', 0.06, i*0.12); }); }
  function sCoin() { sfx(1200, 0.07, 'square', 0.08); sfx(1600, 0.1, 'square', 0.06, 0.06); }

  var CHORDS = [
    [220.00, 261.63, 329.63],
    [174.61, 220.00, 261.63],
    [130.81, 164.81, 196.00],
    [196.00, 246.94, 293.66]
  ];
  var STEP = 0.22;

  function musicTick() {
    if (!musicOn || !S.sound) return;
    var c = ensureAC(); if (!c) return;
    var now = c.currentTime;
    if (musicNext < now) musicNext = now + 0.05;
    while (musicNext < now + 0.3) {
      var t = musicNext;
      var bar = Math.floor(musicStep / 8);
      var chord = CHORDS[bar % 4];
      var pos = musicStep % 8;
      if (pos === 0 || pos === 4) note(chord[0] / 2, t, STEP * 2.2, 'sawtooth', 0.06);
      note(chord[pos % 3] * 2, t, STEP * 0.85, 'triangle', 0.03);
      if (pos % 2 === 1) note(chord[(pos+1) % 3] * 4, t, STEP * 0.5, 'sine', 0.02);
      musicStep++;
      musicNext += STEP;
    }
    requestAnimationFrame(musicTick);
  }
  function startMusic() {
    if (musicOn) return;
    musicOn = true;
    musicNext = (ac ? ac.currentTime : 0) + 0.05;
    requestAnimationFrame(musicTick);
  }
  function stopMusic() { musicOn = false; }

  document.addEventListener('visibilitychange', function() {
    if (document.hidden) stopMusic();
    else if (S.sound) { ensureAC(); startMusic(); }
  });

  /* ============================================
     УТИЛИТЫ
  ============================================ */
  function randSym() {
    var tot = 22+22+18+13+11+8;
    var r = Math.random() * tot;
    for (var i = 0; i < SYMBOLS.length; i++) {
      r -= WEIGHTS[i];
      if (r <= 0) return SYMBOLS[i];
    }
    return SYMBOLS[0];
  }

  function pickItemByRarity() {
    var r = Math.random();
    var rarity;
    if (r < 0.01) rarity = 'vasya';
    else if (r < 0.05) rarity = 'legend';
    else if (r < 0.15) rarity = 'epic';
    else if (r < 0.40) rarity = 'rare';
    else rarity = 'common';
    var pool = ITEMS.filter(function(it) { return it.rarity === rarity; });
    return pool[(Math.random() * pool.length) | 0];
  }

  var COLORS = ['#fbbf24','#f472b6','#38bdf8','#4ade80','#a78bfa','#fff'];
  var confettiPool = [];
  function confetti(n) {
    for (var i = 0; i < n; i++) {
      var c;
      if (confettiPool.length) c = confettiPool.pop();
      else { c = document.createElement('div'); c.className = 'confetti'; }
      c.style.left = (Math.random() * 100) + 'vw';
      c.style.background = COLORS[(Math.random() * 6) | 0];
      c.style.width = (5 + Math.random() * 6) + 'px';
      c.style.height = (8 + Math.random() * 8) + 'px';
      c.style.animationDuration = (1.3 + Math.random() * 1.2) + 's';
      c.style.animationDelay = (Math.random() * 0.3) + 's';
      document.body.appendChild(c);
      (function(el) {
        setTimeout(function() {
          if (el.parentNode) el.parentNode.removeChild(el);
          if (confettiPool.length < 80) confettiPool.push(el);
        }, 2800);
      })(c);
    }
  }
  function coins(fromEl) {
    if (!fromEl) return;
    var rect = fromEl.getBoundingClientRect();
    var cx = rect.left + rect.width / 2, cy = rect.top + rect.height / 2;
    for (var i = 0; i < 5; i++) {
      var c = document.createElement('div');
      c.className = 'coin';
      c.textContent = '🪙';
      c.style.left = (cx + (Math.random() - 0.5) * 50) + 'px';
      c.style.top = cy + 'px';
      c.style.animationDelay = (i * 0.05) + 's';
      document.body.appendChild(c);
      (function(el) {
        setTimeout(function() { if (el.parentNode) el.parentNode.removeChild(el); }, 1100);
      })(c);
    }
  }

  /* ============================================
     RENDER
  ============================================ */
  var lastBalance = -1;
  function render() {
    if (S.balance !== lastBalance) {
      balVal.textContent = S.balance.toLocaleString('ru-RU');
      lastBalance = S.balance;
    }
    betVal.textContent = S.bet;
    winsEl.textContent = S.wins;
    spinsEl.textContent = S.spins;
    jpEl.textContent = S.jackpots;
    sndBtn.textContent = S.sound ? '🔊' : '🔇';
    sndBtn.classList.toggle('off', !S.sound);
  }
  function bumpBal() {
    balEl.classList.remove('bump');
    void balEl.offsetWidth;
    balEl.classList.add('bump');
  }
  function addMoney(amount) {
    S.balance += amount;
    render(); bumpBal(); save();
    checkAchievements();
  }

  /* ============================================
     ДОСТИЖЕНИЯ
  ============================================ */
  function checkAchievements() {
    var changed = false;
    for (var i = 0; i < ACHIEVEMENTS.length; i++) {
      var a = ACHIEVEMENTS[i];
      if (S.unlockedAch.indexOf(a.id) === -1 && a.check(S)) {
        S.unlockedAch.push(a.id);
        changed = true;
        (function(ach) {
          setTimeout(function() { showAchUnlock(ach); }, 400);
        })(a);
      }
    }
    if (changed) { save(); renderAch(); }
  }

  function showAchUnlock(ach) {
    var el = document.createElement('div');
    el.style.cssText = 'position:fixed;top:60px;left:50%;transform:translateX(-50%);background:linear-gradient(160deg,#2e1a44,#12071e);border:3px solid #4ade80;border-radius:14px;padding:14px 18px;z-index:3000;text-align:center;box-shadow:0 0 30px rgba(74,222,128,0.6);animation:pop 0.5s ease;max-width:90%;';
    el.innerHTML = '<div style="font-size:32px">' + ach.icon + '</div><div style="color:#4ade80;font-weight:900;font-size:13px;margin-top:4px">ДОСТИЖЕНИЕ!</div><div style="color:#fff;font-size:12px;margin-top:2px">' + ach.name + '</div>';
    document.body.appendChild(el);
    sJack(); haptic('success');
    setTimeout(function() { if (el.parentNode) el.parentNode.removeChild(el); }, 3000);
  }

  function renderAch() {
    var list = $('achList');
    list.innerHTML = '';
    for (var i = 0; i < ACHIEVEMENTS.length; i++) {
      var a = ACHIEVEMENTS[i];
      var unlocked = S.unlockedAch.indexOf(a.id) !== -1;
      var el = document.createElement('div');
      el.className = 'ach-item' + (unlocked ? ' unlocked' : '');
      el.innerHTML = '<div class="ach-icon">' + a.icon + '</div>' +
                     '<div class="ach-info"><div class="ach-name">' + a.name + '</div>' +
                     '<div class="ach-desc">' + a.desc + '</div></div>' +
                     '<div class="ach-progress">' + a.progress(S) + '</div>';
      list.appendChild(el);
    }
    $('achCount').textContent = S.unlockedAch.length + ' / ' + ACHIEVEMENTS.length;
  }

  /* ============================================
     ЖИВАЯ ЛЕНТА
  ============================================ */
  var FAKE_NAMES = ['Вася228','Мария','Kolya_X','Олег','SlavaPro','Аня⭐','Дмитрий','Kote_king','Инна','Максим2000','Лена','Barabashka','Сергей','Mr.Cat','Юля','Роман','НикПро','Алина','Владимир','XxX_Игорь_XxX','Настя','Кирилл','Полина','Тимоха'];

  var FAKE_WINS = [
    { sym: '🍒', mult: 5 }, { sym: '🍋', mult: 8 }, { sym: '🔔', mult: 15 },
    { sym: '💎', mult: 30 }, { sym: '7️⃣', mult: 50 }, { sym: '👨', mult: 200 },
    { sym: '🥇', mult: 500 }, { sym: '👑', mult: 2000 }
  ];

  var MAX_FEED = 14;
  var feedItems = [];

  function makeFeedItem(name, sym, mult, bet) {
    var payout = (bet || 10) * mult;
    var el = document.createElement('div');
    el.className = 'feed-item';
    el.innerHTML = '<span class="f-sym">' + sym + '</span>' +
                   '<span class="f-name"></span>' +
                   '<span class="f-x">×' + mult + '</span>' +
                   '<span class="f-payout">+' + payout.toLocaleString('ru-RU') + '</span>';
    el.querySelector('.f-name').textContent = name;
    return el;
  }

  function pushFeed(name, sym, mult, bet) {
    var el = makeFeedItem(name, sym, mult, bet);
    feedItems.push(el);
    while (feedItems.length > MAX_FEED) {
      var old = feedItems.shift();
      if (old.parentNode) old.parentNode.removeChild(old);
    }
    feedTrack.appendChild(el);
    rebuildFeedClones();
  }

  function rebuildFeedClones() {
    var clones = feedTrack.querySelectorAll('.feed-clone');
    for (var i = 0; i < clones.length; i++) clones[i].remove();
    for (var j = 0; j < feedItems.length; j++) {
      var c = feedItems[j].cloneNode(true);
      c.classList.add('feed-clone');
      feedTrack.appendChild(c);
    }
  }

  var feedX = 0, lastFeedTime = 0, feedSpeed = 0.03, feedWidth = 0;

  function feedLoop(now) {
    var dt = Math.min(50, now - lastFeedTime);
    lastFeedTime = now;
    feedWidth = feedTrack.scrollWidth / 2;
    if (feedWidth > 0) {
      feedX -= feedSpeed * dt;
      if (feedX <= -feedWidth) feedX += feedWidth;
      feedTrack.style.transform = 'translate3d(' + feedX + 'px,0,0)';
    }
    requestAnimationFrame(feedLoop);
  }

  function pickFakeWin() {
    var r = Math.random();
    if (r < 0.001) return FAKE_WINS[7];
    if (r < 0.01)  return FAKE_WINS[6];
    if (r < 0.05)  return FAKE_WINS[5];
    if (r < 0.15)  return FAKE_WINS[4];
    if (r < 0.30)  return FAKE_WINS[3];
    if (r < 0.55)  return FAKE_WINS[2];
    if (r < 0.80)  return FAKE_WINS[1];
    return FAKE_WINS[0];
  }

  function spawnFakeEvent() {
    var name = FAKE_NAMES[(Math.random() * FAKE_NAMES.length) | 0];
    var win = pickFakeWin();
    var bet = BETS[(Math.random() * 5) | 0];
    pushFeed(name, win.sym, win.mult, bet);
  }
  function scheduleFakeEvent() {
    setTimeout(function() { spawnFakeEvent(); scheduleFakeEvent(); }, 2500 + Math.random() * 3000);
  }
  function initFeed() {
    for (var i = 0; i < 8; i++) spawnFakeEvent();
    requestAnimationFrame(function(t) {
      lastFeedTime = t;
      requestAnimationFrame(feedLoop);
    });
    scheduleFakeEvent();
  }

  /* ============================================
     СЛОТ
  ============================================ */
  function outcome() {
    var r = Math.random();
    if (r < 0.003) return [LEGEND, LEGEND, LEGEND];
    if (r < 0.015) return [GOLD, GOLD, GOLD];
    if (r < 0.04)  return [VASYA, VASYA, VASYA];
    if (r < 0.18) {
      var pool = SYMBOLS.filter(function(s){ return s !== VASYA; });
      var s = pool[(Math.random() * pool.length) | 0];
      return [s, s, s];
    }
    var f;
    do { f = [randSym(), randSym(), randSym()]; }
    while (f[0] === f[1] && f[1] === f[2]);
    return f;
  }

  function evaluate(f, bet) {
    var same = f[0] === f[1] && f[1] === f[2];
    if (same && f[0] === LEGEND) return { t: 'legend', p: PAY_LEGEND * bet, m: '👑 ЛЕГЕНДАРНЫЙ ВАСЯ! ×' + PAY_LEGEND, s: LEGEND, mult: PAY_LEGEND };
    if (same && f[0] === GOLD)   return { t: 'gold',   p: PAY_GOLD * bet,   m: '🥇 ЗОЛОТОЙ ВАСЯ! ×' + PAY_GOLD, s: GOLD, mult: PAY_GOLD };
    if (same && f[0] === VASYA)  return { t: 'jackpot',p: PAYOUTS[VASYA] * bet, m: '👨 Вам выпал Вася! ×' + PAYOUTS[VASYA], s: VASYA, mult: PAYOUTS[VASYA] };
    if (same) {
      var mult = PAYOUTS[f[0]] || 5;
      return { t: 'win', p: mult * bet, m: '🎉 Вам выпал Вася! ×' + mult, s: f[0], mult: mult };
    }
    return { t: 'lose', p: 0, m: '😢 Вам не выпал Вася', s: '❌', mult: 0 };
  }

  function spinReel(strip, finalSym, dur) {
    return new Promise(function(resolve) {
      var count = 12, html = '';
      for (var i = 0; i < count - 1; i++) html += '<div class="sym">' + randSym() + '</div>';
      html += '<div class="sym">' + finalSym + '</div>';
      strip.innerHTML = html;
      strip.style.transition = 'none';
      strip.style.transform = 'translateY(0)';
      void strip.offsetHeight;

      requestAnimationFrame(function() {
        strip.style.transition = 'transform ' + dur + 'ms cubic-bezier(0.15, 0.9, 0.25, 1)';
        strip.style.transform = 'translate3d(0, -' + (count - 1) * SYM_H + 'px, 0)';
      });

      setTimeout(function() {
        strip.style.transition = 'none';
        strip.innerHTML = '<div class="sym">' + finalSym + '</div>';
        strip.style.transform = 'translate3d(0, 0, 0)';
        sTick(); haptic('light');
        resolve();
      }, dur + 30);
    });
  }

  function spin() {
    if (spinning) return;
    if (S.balance < S.bet) { modal.classList.add('show'); return; }

    spinning = true;
    spinBtn.disabled = true;
    spinBtn.textContent = 'Крутим...';
    resultEl.className = 'result';
    resultEl.textContent = '';
    machine.className = 'machine';

    S.balance -= S.bet;
    render(); bumpBal(); save();

    var finals = outcome();
    var durs = [1100, 1400, 1700];

    Promise.all([
      spinReel(stripEls[0], finals[0], durs[0]),
      spinReel(stripEls[1], finals[1], durs[1]),
      spinReel(stripEls[2], finals[2], durs[2])
    ]).then(function() {
      var res = evaluate(finals, S.bet);
      S.spins++;

      if (res.t === 'lose') {
        resultEl.className = 'result lose';
        resultEl.textContent = res.m;
        machine.className = 'machine lose-glow shake';
        sLose(); haptic('error');
        setTimeout(function() { machine.className = 'machine'; }, 900);
      } else {
        S.balance += res.p;
        S.wins++;
        if (res.t === 'jackpot' || res.t === 'gold' || res.t === 'legend') S.jackpots++;
        if (res.p > S.maxWin) S.maxWin = res.p;

        var nConf = 16, cls = 'result win', glow = 'win-glow';
        if (res.t === 'legend') { cls = 'result jackpot'; nConf = 40; glow = 'jackpot-glow'; }
        else if (res.t === 'gold') { cls = 'result jackpot'; nConf = 28; glow = 'jackpot-glow'; }
        else if (res.t === 'jackpot') { cls = 'result jackpot'; nConf = 24; glow = 'jackpot-glow'; }

        resultEl.className = cls;
        resultEl.innerHTML = res.m + '<div class="sub">+' + res.p.toLocaleString('ru-RU') + ' монет</div>';

        machine.className = 'machine ' + glow;
        setTimeout(function() { machine.className = 'machine'; }, 1800);

        confetti(nConf);
        coins(spinBtn);

        if (res.t === 'win') sWin(); else sJack();
        sCoin(); haptic('success');
        bumpBal();

        pushFeed(S.name || 'Ты', res.s, res.mult, S.bet);
      }

      render(); save();
      checkAchievements();

      spinBtn.textContent = 'Крутить';
      spinBtn.disabled = false;
      spinning = false;

      if (S.balance < S.bet) setTimeout(function() { modal.classList.add('show'); }, 1200);
    });
  }

  /* ============================================
     КЕЙС
  ============================================ */
  function openCase() {
    if (caseSpinning) return;
    if (S.balance < CASE_PRICE) { modal.classList.add('show'); return; }

    caseSpinning = true;
    caseOpenBtn.disabled = true;
    caseResult.className = 'result';
    caseResult.textContent = '';

    S.balance -= CASE_PRICE;
    render(); bumpBal(); save();

    var finalItem = pickItemByRarity();
    var count = 30, winPos = 24, items = [];
    for (var i = 0; i < count; i++) {
      if (i === winPos) items.push(finalItem);
      else items.push(ITEMS[(Math.random() * ITEMS.length) | 0]);
    }

    var html = '';
    for (var k = 0; k < items.length; k++) {
      var it = items[k];
      html += '<div class="case-item r-' + it.rarity + '">' +
              '<div class="ci-icon">' + it.icon + '</div>' +
              '<div class="ci-name">' + it.name + '</div>' +
              '<div class="ci-stripe"></div></div>';
    }
    caseStrip.innerHTML = html;
    caseRoulette.classList.add('show');
    caseStrip.style.transition = 'none';
    caseStrip.style.transform = 'translate3d(0,0,0)';
    void caseStrip.offsetHeight;

    setTimeout(function() {
      var rect = caseRoulette.getBoundingClientRect();
      var centerX = rect.width / 2;
      var itemW = 90;
      var targetOffset = winPos * itemW + (84 / 2) - centerX;
      var jitter = (Math.random() - 0.5) * 30;

      caseStrip.style.transition = 'transform 4500ms cubic-bezier(0.1, 0.75, 0.15, 1)';
      caseStrip.style.transform = 'translate3d(-' + (targetOffset + jitter) + 'px,0,0)';

      var tickCount = 0;
      var tickInterval = setInterval(function() {
        sTick();
        tickCount++;
        if (tickCount > 40) clearInterval(tickInterval);
      }, 100);
    }, 50);

    setTimeout(function() {
      caseRoulette.classList.remove('show');
      S.inventory.push({ uid: Date.now() + Math.random(), id: finalItem.id, icon: finalItem.icon, name: finalItem.name, rarity: finalItem.rarity, price: finalItem.price });
      S.casesOpened++;
      if (finalItem.rarity === 'vasya') S.gotVasya = true;
      save();

      var rar = RARITY[finalItem.rarity];
      caseResult.className = 'result win';
      caseResult.innerHTML = rar.name + ': ' + finalItem.icon + ' ' + finalItem.name +
                             '<div class="sub">+' + finalItem.price.toLocaleString('ru-RU') + ' 💰 если продать</div>';

      if (finalItem.rarity === 'vasya') { confetti(60); sJack(); coins(caseOpenBtn); caseResult.className = 'result jackpot'; haptic('success'); }
      else if (finalItem.rarity === 'legend') { confetti(30); sJack(); haptic('success'); }
      else if (finalItem.rarity === 'epic') { confetti(15); sWin(); haptic('medium'); }
      else { sCoin(); haptic('light'); }

      pushFeed(S.name || 'Ты', finalItem.icon, finalItem.price, 1);

      caseOpenBtn.disabled = false;
      caseSpinning = false;
      render(); renderInv(); checkAchievements();
      if (S.balance < CASE_PRICE) setTimeout(function() { modal.classList.add('show'); }, 1200);
    }, 4600);
  }

  function openCase5() {
    if (caseSpinning) return;
    if (S.balance < CASE_PRICE_5) { modal.classList.add('show'); return; }

    S.balance -= CASE_PRICE_5;
    render(); bumpBal(); save();

    var opened = [];
    for (var i = 0; i < 5; i++) {
      var it = pickItemByRarity();
      S.inventory.push({ uid: Date.now() + Math.random() + i, id: it.id, icon: it.icon, name: it.name, rarity: it.rarity, price: it.price });
      S.casesOpened++;
      if (it.rarity === 'vasya') S.gotVasya = true;
      opened.push(it);
      pushFeed(S.name || 'Ты', it.icon, it.price, 1);
    }
    save();

    var html = '';
    var colorMap = { common: '#9ca3af', rare: '#60a5fa', epic: '#c084fc', legend: '#fbbf24', vasya: '#f472b6' };
    for (var k = 0; k < opened.length; k++) {
      var o = opened[k];
      var rar = RARITY[o.rarity];
      html += '<div style="display:inline-block;margin:4px;text-align:center;font-size:11px">' +
              '<div style="font-size:26px">' + o.icon + '</div>' +
              '<div style="color:' + colorMap[o.rarity] + ';font-weight:900;font-size:9px">' + rar.name + '</div>' +
              '</div>';
    }
    caseResult.className = 'result win';
    caseResult.innerHTML = 'Открыто 5 кейсов:<div class="sub" style="margin-top:6px">' + html + '</div>';

    confetti(20); sJack(); haptic('success');
    render(); renderInv(); checkAchievements();
  }

  function sellDupes() {
    var seen = {}, sold = 0, profit = 0, newInv = [];
    for (var i = 0; i < S.inventory.length; i++) {
      var it = S.inventory[i];
      if (seen[it.id]) { sold++; profit += it.price; }
      else { seen[it.id] = true; newInv.push(it); }
    }
    if (sold === 0) {
      caseResult.className = 'result lose';
      caseResult.textContent = 'Нет дублей для продажи';
      return;
    }
    S.inventory = newInv;
    S.balance += profit;
    render(); bumpBal(); save();
    caseResult.className = 'result win';
    caseResult.innerHTML = 'Продано ' + sold + ' шт.<div class="sub">+' + profit.toLocaleString('ru-RU') + ' 💰</div>';
    sCoin(); coins($('caseSellDupesBtn'));
    renderInv();
  }

  /* ============================================
     ИНВЕНТАРЬ
  ============================================ */
  function renderInv() {
    invGrid.innerHTML = '';
    if (!S.inventory || S.inventory.length === 0) {
      invGrid.innerHTML = '<div class="inv-empty">Инвентарь пуст.<br>Открой кейс Юр Юрыча!</div>';
      invCount.textContent = '0 шт.';
      invTotal.textContent = '0 💰';
      return;
    }
    var total = 0, groups = {};
    for (var i = 0; i < S.inventory.length; i++) {
      var it = S.inventory[i];
      if (!groups[it.id]) groups[it.id] = { item: it, count: 0 };
      groups[it.id].count++;
      total += it.price;
    }
    var keys = Object.keys(groups);
    for (var j = 0; j < keys.length; j++) {
      var g = groups[keys[j]];
      var it = g.item;
      var card = document.createElement('div');
      card.className = 'inv-card r-' + it.rarity;
      card.innerHTML =
        '<div class="ic-icon">' + it.icon + '</div>' +
        '<div class="ic-name">' + it.name + (g.count > 1 ? ' ×' + g.count : '') + '</div>' +
        '<div class="ic-price">' + it.price + ' 💰</div>' +
        '<div class="ic-stripe"></div>';
      (function(item) {
        card.addEventListener('click', function() { showItemModal(item); });
      })(it);
      invGrid.appendChild(card);
    }
    invCount.textContent = S.inventory.length + ' шт.';
    invTotal.textContent = total.toLocaleString('ru-RU') + ' 💰';
  }

  function showItemModal(item) {
    currentItem = item;
    var rar = RARITY[item.rarity];
    $('imIcon').textContent = item.icon;
    $('imName').textContent = item.name;
    $('imRarity').textContent = rar.name;
    $('imRarity').className = 'item-rarity ' + rar.cls;
    $('imPrice').textContent = '+' + item.price + ' 💰';
    itemModal.classList.add('show');
  }

  function sellCurrentItem() {
    if (!currentItem) return;
    for (var i = 0; i < S.inventory.length; i++) {
      if (S.inventory[i].uid === currentItem.uid) { S.inventory.splice(i, 1); break; }
    }
    S.balance += currentItem.price;
    render(); bumpBal(); save();
    itemModal.classList.remove('show');
    sCoin(); coins($('imSellBtn'));
    renderInv();
  }

  function sendToUpgrade() {
    if (!currentItem) return;
    upgMyItem = currentItem;
    itemModal.classList.remove('show');
    switchPage('upg');
    updateUpgradeUI();
  }

  function sellAllItems() {
    if (!S.inventory.length) return;
    var total = 0;
    for (var i = 0; i < S.inventory.length; i++) total += S.inventory[i].price;
    S.inventory = [];
    S.balance += total;
    render(); bumpBal(); save();
    renderInv();
    alert('Продано всё за ' + total.toLocaleString('ru-RU') + ' 💰');
  }

  /* ============================================
     АПГРЕЙД
  ============================================ */
  var upgMyItem = null, upgTargetItem = null, upgInProgress = false, upgPickerMode = '';
  var upgMyContent = $('upgMyContent'), upgTargetContent = $('upgTargetContent'),
      upgChanceVal = $('upgChanceVal'), upgBtn = $('upgBtn'), upgResult = $('upgResult'),
      upgRouletteFill = $('upgRouletteFill'), upgRouletteMarker = $('upgRouletteMarker'),
      upgRouletteTarget = $('upgRouletteTarget'), upgPicker = $('upgPicker'),
      upgPickerGrid = $('upgPickerGrid'), upgPickerTitle = $('upgPickerTitle');

  function calcUpgradeChance() {
    if (!upgMyItem || !upgTargetItem) return 0;
    var ratio = upgMyItem.price / upgTargetItem.price;
    if (ratio >= 1) return 90;
    var chance = Math.round(ratio * 100);
    if (chance < 5) chance = 5;
    if (chance > 90) chance = 90;
    return chance;
  }

  function updateUpgradeUI() {
    if (upgMyItem) {
      upgMyContent.className = 'upg-slot-content has-item r-' + upgMyItem.rarity;
      upgMyContent.innerHTML = '<div class="upg-item-icon">' + upgMyItem.icon + '</div>' +
        '<div class="upg-item-name">' + upgMyItem.name + '</div>' +
        '<div class="upg-item-price">' + upgMyItem.price + ' 💰</div>';
    } else {
      upgMyContent.className = 'upg-slot-content';
      upgMyContent.innerHTML = '<div class="upg-empty">Нажми, чтобы выбрать</div>';
    }
    if (upgTargetItem) {
      upgTargetContent.className = 'upg-slot-content has-item r-' + upgTargetItem.rarity;
      upgTargetContent.innerHTML = '<div class="upg-item-icon">' + upgTargetItem.icon + '</div>' +
        '<div class="upg-item-name">' + upgTargetItem.name + '</div>' +
        '<div class="upg-item-price">' + upgTargetItem.price + ' 💰</div>';
    } else {
      upgTargetContent.className = 'upg-slot-content';
      upgTargetContent.innerHTML = '<div class="upg-empty">Нажми, чтобы выбрать</div>';
    }
    var realChance = calcUpgradeChance();
    upgChanceVal.textContent = realChance > 0 ? realChance + '%' : '—';
    upgRouletteFill.style.width = realChance + '%';
    upgRouletteTarget.style.left = realChance + '%';
    upgBtn.disabled = !(upgMyItem && upgTargetItem && !upgInProgress && realChance > 0);
  }

  function openUpgradePicker(mode) {
    upgPickerMode = mode;
    upgPickerTitle.textContent = mode === 'my' ? 'Выбери свой предмет' : 'Выбери цель';
    upgPickerGrid.innerHTML = '';

    if (mode === 'my') {
      if (!S.inventory.length) {
        upgPickerGrid.innerHTML = '<div class="inv-empty" style="grid-column:1/-1">Инвентарь пуст</div>';
      } else {
        var groups = {};
        for (var i = 0; i < S.inventory.length; i++) {
          var it = S.inventory[i];
          if (!groups[it.id]) groups[it.id] = { item: it, count: 0 };
          groups[it.id].count++;
        }
        var keys = Object.keys(groups);
        for (var j = 0; j < keys.length; j++) {
          var g = groups[keys[j]];
          var card = document.createElement('div');
          card.className = 'inv-card r-' + g.item.rarity;
          card.innerHTML = '<div class="ic-icon">' + g.item.icon + '</div>' +
            '<div class="ic-name">' + g.item.name + (g.count > 1 ? ' ×' + g.count : '') + '</div>' +
            '<div class="ic-price">' + g.item.price + ' 💰</div>' +
            '<div class="ic-stripe"></div>';
          (function(item) {
            card.addEventListener('click', function() {
              upgMyItem = item;
              upgPicker.classList.remove('show');
              updateUpgradeUI();
            });
          })(g.item);
          upgPickerGrid.appendChild(card);
        }
      }
    } else {
      for (var k = 0; k < ITEMS.length; k++) {
        var item = ITEMS[k];
        var card2 = document.createElement('div');
        card2.className = 'inv-card r-' + item.rarity;
        card2.innerHTML = '<div class="ic-icon">' + item.icon + '</div>' +
          '<div class="ic-name">' + item.name + '</div>' +
          '<div class="ic-price">' + item.price + ' 💰</div>' +
          '<div class="ic-stripe"></div>';
        (function(it) {
          card2.addEventListener('click', function() {
            upgTargetItem = it;
            upgPicker.classList.remove('show');
            updateUpgradeUI();
          });
        })(item);
        upgPickerGrid.appendChild(card2);
      }
    }
    upgPicker.classList.add('show');
  }

  function startUpgrade() {
    if (upgInProgress || !upgMyItem || !upgTargetItem) return;

    var realChance = calcUpgradeChance();
    upgInProgress = true;
    upgBtn.disabled = true;
    upgResult.className = 'result';
    upgResult.textContent = '';

    var success = Math.random() * 100 <= realChance;
    var finalPos;
    if (success) finalPos = Math.random() * realChance;
    else finalPos = realChance + Math.random() * (100 - realChance - 1);

    var startTime = performance.now();
    var duration = 2500;

    function animateMarker(now) {
      var elapsed = now - startTime;
      var progress = Math.min(1, elapsed / duration);
      if (progress < 1) {
        var pos = (Math.sin(elapsed / 80) * 0.5 + 0.5) * 100;
        upgRouletteMarker.style.left = pos + '%';
        if (Math.random() < 0.3) sTick();
        requestAnimationFrame(animateMarker);
      } else {
        upgRouletteMarker.style.left = finalPos + '%';
        setTimeout(function() {
          if (success) {
            for (var i = 0; i < S.inventory.length; i++) {
              if (S.inventory[i].uid === upgMyItem.uid) { S.inventory.splice(i, 1); break; }
            }
            S.inventory.push({
              uid: Date.now() + Math.random(),
              id: upgTargetItem.id, icon: upgTargetItem.icon,
              name: upgTargetItem.name, rarity: upgTargetItem.rarity,
              price: upgTargetItem.price
            });
            save(); renderInv();
            upgResult.className = 'result win';
            upgResult.innerHTML = '✅ УСПЕХ! Получен ' + upgTargetItem.icon + ' ' + upgTargetItem.name +
              '<div class="sub">+' + (upgTargetItem.price - upgMyItem.price).toLocaleString('ru-RU') + ' 💰 профит</div>';
            if (upgTargetItem.rarity === 'vasya' || upgTargetItem.rarity === 'legend') { confetti(40); sJack(); haptic('success'); }
            else { confetti(18); sWin(); haptic('medium'); }
            coins(upgBtn);
            pushFeed(S.name || 'Ты', upgTargetItem.icon, upgTargetItem.price, 1);
            upgMyItem = null; upgTargetItem = null;
            updateUpgradeUI();
            upgInProgress = false;
          } else {
            for (var j = 0; j < S.inventory.length; j++) {
              if (S.inventory[j].uid === upgMyItem.uid) { S.inventory.splice(j, 1); break; }
            }
            save(); renderInv();
            upgResult.className = 'result lose';
            upgResult.innerHTML = '💀 ПРОВАЛ! Потерян ' + upgMyItem.icon + ' ' + upgMyItem.name +
              '<div class="sub">Шанс был ' + realChance + '%</div>';
            sLose(); haptic('error');
            upgMyItem = null;
            updateUpgradeUI();
            upgInProgress = false;
          }
          checkAchievements();
        }, 200);
      }
    }
    requestAnimationFrame(animateMarker);
  }

  /* ============================================
     ЕЖЕДНЕВНЫЙ БОНУС
  ============================================ */
  function updateDaily() {
    var now = Date.now();
    var elapsed = now - S.lastDaily;
    var btn = $('dailyBtn'), timer = $('dailyTimer'), desc = $('dailyDesc');
    if (elapsed >= DAILY_CD) {
      btn.disabled = false;
      btn.textContent = '🎁 ЗАБРАТЬ 500';
      timer.textContent = 'Готово!';
      timer.style.color = '#4ade80';
      desc.textContent = 'Забирай 500 монет каждые 24 часа';
    } else {
      var left = DAILY_CD - elapsed;
      var h = Math.floor(left / 3600000);
      var m = Math.floor((left % 3600000) / 60000);
      var sec = Math.floor((left % 60000) / 1000);
      btn.disabled = true;
      btn.textContent = '⏳ УЖЕ ЗАБРАНО';
      timer.textContent = (h < 10 ? '0' : '') + h + ':' + (m < 10 ? '0' : '') + m + ':' + (sec < 10 ? '0' : '') + sec;
      timer.style.color = '#a78bfa';
      desc.textContent = 'Приходи позже';
    }
  }
  setInterval(updateDaily, 1000);

  function claimDaily() {
    if (Date.now() - S.lastDaily < DAILY_CD) return;
    S.lastDaily = Date.now();
    S.balance += DAILY_AMOUNT;
    render(); bumpBal(); save();
    sJack(); confetti(25); coins($('dailyBtn')); haptic('success');
    checkAchievements();
    updateDaily();
  }

  /* ============================================
     СКИНЫ
  ============================================ */
  var pendingSkin = null;
  function renderSkins() {
    var grid = $('skinGrid');
    grid.innerHTML = '';
    for (var i = 0; i < SKINS.length; i++) {
      var sk = SKINS[i];
      var unlocked = S.unlockedSkins.indexOf(sk.id) !== -1;
      var active = S.activeSkin === sk.id;
      var card = document.createElement('div');
      card.className = 'skin-card' + (active ? ' active' : '') + (!unlocked ? ' locked' : '');
      card.innerHTML = '<div class="skin-preview" style="background:' + sk.preview + '"></div>' +
        '<div class="skin-name">' + sk.name + (active ? ' ✓' : '') + '</div>' +
        '<div class="skin-cost">' + (unlocked ? (active ? 'АКТИВЕН' : 'ВЫБРАТЬ') : sk.cost.toLocaleString('ru-RU') + ' 💰') + '</div>';
      (function(skin, isUnlocked) {
        card.addEventListener('click', function() {
          if (isUnlocked) {
            S.activeSkin = skin.id;
            applySkin(); save(); renderSkins();
            sCoin(); haptic('light');
          } else {
            pendingSkin = skin;
            $('skinModalTitle').textContent = '🎨 ' + skin.name;
            $('skinModalDesc').textContent = 'Стоимость: ' + skin.cost.toLocaleString('ru-RU') + ' монет. Купить?';
            $('skinModal').classList.add('show');
          }
        });
      })(sk, unlocked);
      grid.appendChild(card);
    }
  }

  function buyPendingSkin() {
    if (!pendingSkin) return;
    if (S.balance < pendingSkin.cost) {
      alert('Не хватает монет! Нужно ' + pendingSkin.cost.toLocaleString('ru-RU'));
      return;
    }
    S.balance -= pendingSkin.cost;
    S.unlockedSkins.push(pendingSkin.id);
    S.activeSkin = pendingSkin.id;
    applySkin();
    render(); save(); renderSkins();
    $('skinModal').classList.remove('show');
    pendingSkin = null;
    confetti(20); sJack(); coins($('skinBuyBtn')); haptic('success');
  }

  /* ============================================
     ПОДЕЛИТЬСЯ В TELEGRAM
  ============================================ */
  function getShareUrl() {
    return window.location.href.split('#')[0].split('?')[0];
  }

  function inviteFriends() {
    var url = getShareUrl();
    var text = '🎰 Заходи в Казино Васи! Крути барабаны, открывай кейсы Юр Юрыча, лови джекпот!';

    if (tg && tg.openTelegramLink) {
      try {
        var tgShare = 'https://t.me/share/url?url=' + encodeURIComponent(url) + '&text=' + encodeURIComponent(text);
        tg.openTelegramLink(tgShare);
        haptic('success');
        return;
      } catch(e) {}
    }

    // fallback
    if (navigator.share) {
      navigator.share({ title: '🎰 Казино Васи', text: text, url: url }).catch(function(){});
    } else {
      var full = text + '\n👉 ' + url;
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(full).then(function() {
          alert('Ссылка скопирована! Отправь друзьям 👇\n\n' + full);
        }).catch(function() {
          prompt('Скопируй и отправь:', full);
        });
      } else {
        prompt('Скопируй и отправь:', full);
      }
    }
    haptic('success');
  }

  function shareRecord() {
    var url = getShareUrl();
    var text;
    if (S.maxWin > 0) {
      text = '🏆 Мой рекорд в Казино Васи — ' + S.maxWin.toLocaleString('ru-RU') + ' монет за один спин! Сможешь больше? 🎰';
    } else {
      text = '🎰 Играю в Казино Васи! Крути барабаны, лови Васю! 🎩';
    }

    if (tg && tg.openTelegramLink) {
      try {
        var tgShare = 'https://t.me/share/url?url=' + encodeURIComponent(url) + '&text=' + encodeURIComponent(text);
        tg.openTelegramLink(tgShare);
        haptic('success');
        return;
      } catch(e) {}
    }

    if (navigator.share) {
      navigator.share({ title: '🎰 Казино Васи', text: text, url: url }).catch(function(){});
    } else {
      var full = text + '\n👉 ' + url;
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(full).then(function() {
          alert('Скопировано! Отправь друзьям 👇\n\n' + full);
        }).catch(function() {
          prompt('Скопируй и отправь:', full);
        });
      } else {
        prompt('Скопируй и отправь:', full);
      }
    }
    haptic('success');
  }

  /* ============================================
     ТАБЫ
  ============================================ */
  function switchPage(name) {
    var pages = document.querySelectorAll('.page');
    for (var i = 0; i < pages.length; i++) {
      pages[i].classList.toggle('active', pages[i].id === 'page-' + name);
    }
    var tabs = document.querySelectorAll('.tab');
    for (var j = 0; j < tabs.length; j++) {
      tabs[j].classList.toggle('active', tabs[j].dataset.page === name);
    }
    if (name === 'inv') renderInv();
    if (name === 'more') { updateDaily(); renderSkins(); renderAch(); }
  }

  /* ============================================
     ОБРАБОТЧИКИ
  ============================================ */
  function on(el, fn) {
    if (el) el.addEventListener('click', function(e) { e.preventDefault(); fn(e); });
  }

  on($('becomeBtn'), function() { showScreen('choice'); haptic('light'); });
  on($('backToStart'), function() { showScreen('start'); });
  on($('goCasino'), function() { showScreen('game'); switchPage('slot'); ensureAC(); if (S.sound) startMusic(); haptic('light'); });
  on($('goCase'), function() { showScreen('game'); switchPage('case'); ensureAC(); if (S.sound) startMusic(); haptic('light'); });

  on(spinBtn, spin);
  on($('bMinus'), function() {
    var i = BETS.indexOf(S.bet);
    if (i > 0) { S.bet = BETS[i - 1]; render(); save(); haptic('light'); }
  });
  on($('bPlus'), function() {
    var i = BETS.indexOf(S.bet);
    if (i >= 0 && i < BETS.length - 1) { S.bet = BETS[i + 1]; render(); save(); haptic('light'); }
  });

  on(sndBtn, function() {
    S.sound = !S.sound;
    render(); save();
    if (S.sound) { ensureAC(); startMusic(); } else stopMusic();
    haptic('light');
  });

  var tabs = document.querySelectorAll('.tab');
  for (var t = 0; t < tabs.length; t++) {
    (function(tab) {
      tab.addEventListener('click', function() { switchPage(tab.dataset.page); haptic('light'); });
    })(tabs[t]);
  }

  on(caseOpenBtn, openCase);
  on($('caseOpen5Btn'), openCase5);
  on($('caseSellDupesBtn'), sellDupes);

  on($('invSellAllBtn'), sellAllItems);
  on($('invCloseBtn'), function() { switchPage('slot'); });
  on($('imSellBtn'), sellCurrentItem);
  on($('imUpgradeBtn'), sendToUpgrade);
  on($('imCloseBtn'), function() { itemModal.classList.remove('show'); });

  on(upgMyContent, function() { if (!upgInProgress) openUpgradePicker('my'); });
  on(upgTargetContent, function() { if (!upgInProgress) openUpgradePicker('target'); });
  on($('upgPickerClose'), function() { upgPicker.classList.remove('show'); });
  on(upgBtn, startUpgrade);

  on($('dailyBtn'), claimDaily);
  on($('skinBuyBtn'), buyPendingSkin);
  on($('skinCancelBtn'), function() { $('skinModal').classList.remove('show'); pendingSkin = null; });

  on($('inviteBtn'), inviteFriends);
  on($('shareRecordBtn'), shareRecord);

  on($('bonusBtn'), function() { addMoney(500); modal.classList.remove('show'); sCoin(); });
  on($('resetBtn2'), function() {
    if (confirm('Сбросить прогресс?')) {
      var nm = S.name, sd = S.sound, sk = S.activeSkin, usk = S.unlockedSkins;
      for (var k in DEF) S[k] = DEF[k];
      S.name = nm; S.sound = sd; S.activeSkin = sk; S.unlockedSkins = usk || ['classic'];
      S.inventory = []; S.unlockedAch = [];
      applySkin(); save(); render(); renderInv(); renderAch(); renderSkins();
      modal.classList.remove('show');
    }
  });

  on($('menuBtn'), function() { menuModal.classList.add('show'); });
  on($('menuClose'), function() { menuModal.classList.remove('show'); });
  on($('menuChangeName'), function() {
    menuModal.classList.remove('show');
    nameInput.value = S.name || '';
    nameModal.classList.add('show');
  });
  on($('menuInvite'), function() {
    menuModal.classList.remove('show');
    inviteFriends();
  });
  on($('menuChangeMode'), function() {
    menuModal.classList.remove('show');
    stopMusic();
    showScreen('choice');
  });

  on($('nameSave'), function() {
    var n = (nameInput.value || '').trim().slice(0, 16);
    if (!n) { nameInput.focus(); return; }
    S.name = n; save();
    nameModal.classList.remove('show');
  });
  on($('nameCancel'), function() { nameModal.classList.remove('show'); });
  nameInput.addEventListener('keydown', function(e) {
    if (e.key === 'Enter') $('nameSave').click();
  });

  document.addEventListener('touchstart', function() { ensureAC(); }, { once: true, passive: true });
  document.addEventListener('click', function() { ensureAC(); }, { once: true });

  /* ============================================
     СТАРТ
  ============================================ */
  render();
  renderInv();
  renderAch();
  renderSkins();
  updateDaily();
  initFeed();

  if (!S.name) {
    setTimeout(function() {
      nameInput.value = '';
      nameModal.classList.add('show');
    }, 1000);
  }
})();
</script>
</body>
</html>
