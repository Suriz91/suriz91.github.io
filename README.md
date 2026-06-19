<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Secant → Tangent Slope | Interactive Demo</title>
    <style>
        /* ---------- Reset & Font ---------- */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background: linear-gradient(145deg, #f0f4f8 0%, #e6ecf3 100%);
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            margin: 0;
        }

        /* ---------- Main Container ---------- */
        .container {
            max-width: 1100px;
            width: 100%;
            background: rgba(255, 255, 255, 0.80);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 28px 30px 32px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25),
                        0 0 0 1px rgba(255, 255, 255, 0.5) inset;
            transition: all 0.2s;
        }

        /* ---------- Header ---------- */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: flex-end;
            flex-wrap: wrap;
            margin-bottom: 18px;
            gap: 10px 20px;
        }
        .header h1 {
            font-size: 26px;
            font-weight: 700;
            color: #0b1a2b;
            letter-spacing: -0.5px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .header h1 small {
            font-size: 16px;
            font-weight: 400;
            color: #4a5b6e;
            letter-spacing: 0;
        }
        .header .badge {
            font-size: 14px;
            background: #1a2b3f;
            color: #eef4fa;
            padding: 5px 16px;
            border-radius: 40px;
            font-weight: 500;
            letter-spacing: 0.3px;
            white-space: nowrap;
        }

        /* ---------- Canvas ---------- */
        .canvas-wrapper {
            position: relative;
            background: #ffffff;
            border-radius: 20px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
            overflow: hidden;
            border: 1px solid rgba(200, 212, 225, 0.4);
            touch-action: none;
        }
        canvas {
            display: block;
            width: 100%;
            height: auto;
            aspect-ratio: 1100 / 620;
            background: #ffffff;
            cursor: default;
        }

        /* ---------- Controls ---------- */
        .controls {
            display: grid;
            grid-template-columns: 1fr 1.2fr;
            gap: 18px 28px;
            margin-top: 20px;
            padding: 18px 20px 14px;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 18px;
            border: 1px solid rgba(200, 212, 225, 0.3);
        }

        .control-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }
        .control-group label {
            font-size: 14px;
            font-weight: 600;
            color: #1e2f40;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .control-group label .value {
            font-weight: 700;
            color: #0b1a2b;
            background: #e9eef4;
            padding: 0 12px;
            border-radius: 30px;
            font-size: 14px;
            min-width: 52px;
            text-align: center;
            font-variant-numeric: tabular-nums;
        }
        input[type="range"] {
            width: 100%;
            height: 6px;
            background: #d0dae6;
            border-radius: 10px;
            outline: none;
            -webkit-appearance: none;
            appearance: none;
            transition: background 0.2s;
        }
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            background: #1a2b3f;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
            border: 2px solid white;
            transition: 0.12s;
        }
        input[type="range"]::-webkit-slider-thumb:hover {
            transform: scale(1.12);
            background: #0f1e2e;
        }
        input[type="range"]::-moz-range-thumb {
            width: 20px;
            height: 20px;
            background: #1a2b3f;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }
        input[type="range"]:focus {
            background: #b8c6d8;
        }

        /* ---------- Row 2: Function + Buttons ---------- */
        .controls-row2 {
            grid-column: 1 / -1;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 12px 20px;
            padding-top: 6px;
            border-top: 1px solid rgba(200, 212, 225, 0.3);
        }
        .func-selector {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .func-selector select {
            padding: 6px 18px 6px 14px;
            border-radius: 40px;
            border: 1px solid #c8d4e1;
            background: white;
            font-size: 15px;
            font-weight: 500;
            color: #0b1a2b;
            cursor: pointer;
            outline: none;
            transition: 0.15s;
            font-family: inherit;
        }
        .func-selector select:hover {
            border-color: #8a9bb0;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
        }
        .func-selector select:focus {
            border-color: #1a2b3f;
            box-shadow: 0 0 0 3px rgba(26, 43, 63, 0.15);
        }

        .btn-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        .btn {
            padding: 7px 22px;
            border-radius: 40px;
            border: none;
            font-weight: 600;
            font-size: 14px;
            cursor: pointer;
            transition: 0.15s;
            background: #e9eef4;
            color: #1a2b3f;
            font-family: inherit;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }
        .btn:hover {
            background: #d5dfeb;
            transform: translateY(-1px);
        }
        .btn-primary {
            background: #1a2b3f;
            color: white;
        }
        .btn-primary:hover {
            background: #0f1e2e;
            box-shadow: 0 4px 14px rgba(26, 43, 63, 0.25);
        }
        .btn-primary.active {
            background: #b22234;
        }
        .btn-primary.active:hover {
            background: #8a1a1c;
        }
        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none !important;
        }

        /* ---------- Statistics ---------- */
        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 8px 20px;
            width: 100%;
            margin-top: 4px;
        }
        .stat-item {
            display: flex;
            align-items: baseline;
            gap: 6px;
            font-size: 14px;
            color: #2d4055;
            background: rgba(255, 255, 255, 0.5);
            padding: 4px 12px 4px 10px;
            border-radius: 30px;
            white-space: nowrap;
        }
        .stat-item .label {
            font-weight: 500;
            color: #4a5b6e;
        }
        .stat-item .num {
            font-weight: 700;
            font-variant-numeric: tabular-nums;
            color: #0b1a2b;
        }
        .stat-item .num.tangent {
            color: #c0392b;
        }
        .stat-item .num.secant {
            color: #2471a3;
        }
        .stat-item .num.diff {
            color: #7d8b9e;
        }
        .stat-item .num.diff-close {
            color: #27ae60;
        }
        .stat-item .badge-diff {
            font-size: 12px;
            font-weight: 600;
            padding: 1px 10px;
            border-radius: 30px;
            background: #e9eef4;
            color: #1a2b3f;
        }

        /* ---------- Responsive ---------- */
        @media (max-width: 720px) {
            .container {
                padding: 16px;
            }
            .controls {
                grid-template-columns: 1fr;
                gap: 14px;
                padding: 14px;
            }
            .controls-row2 {
                flex-direction: column;
                align-items: stretch;
            }
            .func-selector {
                justify-content: center;
            }
            .btn-group {
                justify-content: center;
            }
            .header h1 {
                font-size: 20px;
            }
            .header h1 small {
                font-size: 13px;
            }
            .stats {
                grid-template-columns: 1fr 1fr;
            }
        }
        @media (max-width: 480px) {
            .stats {
                grid-template-columns: 1fr;
            }
            .stat-item {
                white-space: normal;
            }
        }

        /* ---------- Hint ---------- */
        .hint {
            font-size: 13px;
            color: #5a6f84;
            margin-top: 8px;
            text-align: center;
            letter-spacing: 0.2px;
            border-top: 1px solid rgba(200, 212, 225, 0.2);
            padding-top: 10px;
        }
        .hint strong {
            color: #1a2b3f;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- Header -->
        <div class="header">
            <h1>
                📐 Secant → Tangent
                <small>as h → 0</small>
            </h1>
            <span class="badge">⚡ Interactive Demo</span>
        </div>

        <!-- Canvas -->
        <div class="canvas-wrapper">
            <canvas id="canvas" width="1100" height="620"></canvas>
        </div>

        <!-- Controls -->
        <div class="controls">
            <!-- h control -->
            <div class="control-group">
                <label>
                    <span>Interval <i>h</i></span>
                    <span class="value" id="hDisplay">0.80</span>
                </label>
                <input type="range" id="hSlider" min="-3" max="0" step="0.005" value="-0.3" />
                <div style="display:flex;justify-content:space-between;font-size:12px;color:#7a8b9e;padding:0 2px;">
                    <span>large</span>
                    <span>small</span>
                </div>
            </div>

            <!-- x₀ control -->
            <div class="control-group">
                <label>
                    <span>Point <i>x₀</i></span>
                    <span class="value" id="x0Display">1.00</span>
                </label>
                <input type="range" id="x0Slider" min="-1.2" max="2.8" step="0.01" value="1.0" />
                <div style="display:flex;justify-content:space-between;font-size:12px;color:#7a8b9e;padding:0 2px;">
                    <span>−1.2</span>
                    <span>2.8</span>
                </div>
            </div>

            <!-- Row 2: Function + Buttons + Stats -->
            <div class="controls-row2">
                <div class="func-selector">
                    <label style="font-weight:500;color:#1e2f40;">Function</label>
                    <select id="funcSelect">
                        <option value="square">f(x) = x²</option>
                        <option value="sin">f(x) = sin(x)</option>
                        <option value="exp">f(x) = eˣ</option>
                    </select>
                </div>

                <div class="btn-group">
                    <button class="btn btn-primary" id="autoBtn">▶ Auto Demo</button>
                    <button class="btn" id="resetBtn">⟲ Reset</button>
                </div>
            </div>

            <!-- Statistics -->
            <div class="stats" id="statsContainer">
                <div class="stat-item">
                    <span class="label">Tangent slope</span>
                    <span class="num tangent" id="tangentSlope">2.000</span>
                </div>
                <div class="stat-item">
                    <span class="label">Secant slope</span>
                    <span class="num secant" id="secantSlope">2.800</span>
                </div>
                <div class="stat-item">
                    <span class="label">Difference</span>
                    <span class="num diff" id="diffValue">0.800</span>
                    <span class="badge-diff" id="diffPercent">40.0%</span>
                </div>
                <div class="stat-item" style="grid-column:span 1;">
                    <span class="label">Current h</span>
                    <span class="num" id="hStat">0.80</span>
                </div>
            </div>
        </div>

        <div class="hint">
            <strong>💡 Drag the sliders</strong> to change the interval <i>h</i> and observe the secant line (blue) approaching the tangent line (red).
            As <i>h</i> → 0, the secant slope converges to the tangent slope.
        </div>
    </div>

    <script>
        // ============================================================
        // 1. DOM refs
        // ============================================================
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        const hSlider = document.getElementById('hSlider');
        const x0Slider = document.getElementById('x0Slider');
        const hDisplay = document.getElementById('hDisplay');
        const x0Display = document.getElementById('x0Display');
        const funcSelect = document.getElementById('funcSelect');
        const autoBtn = document.getElementById('autoBtn');
        const resetBtn = document.getElementById('resetBtn');

        const tangentSlopeEl = document.getElementById('tangentSlope');
        const secantSlopeEl = document.getElementById('secantSlope');
        const diffValueEl = document.getElementById('diffValue');
        const diffPercentEl = document.getElementById('diffPercent');
        const hStatEl = document.getElementById('hStat');

        // ============================================================
        // 2. Math functions
        // ============================================================
        const FUNCS = {
            square: {
                label: 'x²',
                f: x => x * x,
                df: x => 2 * x,
                secantSlope: (x0, h) => 2 * x0 + h,
                color: '#1a2b3f',
                rangeX: [-1.2, 3.2],
                rangeY: [-0.5, 8.5],
            },
            sin: {
                label: 'sin(x)',
                f: x => Math.sin(x),
                df: x => Math.cos(x),
                secantSlope: (x0, h) => (Math.sin(x0 + h) - Math.sin(x0)) / h,
                color: '#8e44ad',
                rangeX: [-1.2, 4.0],
                rangeY: [-1.8, 1.8],
            },
            exp: {
                label: 'eˣ',
                f: x => Math.exp(x),
                df: x => Math.exp(x),
                secantSlope: (x0, h) => (Math.exp(x0 + h) - Math.exp(x0)) / h,
                color: '#1a7a5a',
                rangeX: [-1.2, 2.8],
                rangeY: [-0.5, 12],
            },
        };

        // ============================================================
        // 3. State
        // ============================================================
        let state = {
            funcKey: 'square',
            x0: 1.0,
            h: 0.80,
            autoMode: false,
            autoTimer: null,
        };

        let W = 1100;
        let H = 620;
        const MARGIN = { top: 32, bottom: 42, left: 52, right: 28 };

        // ============================================================
        // 4. Coordinate transforms
        // ============================================================
        function getRange() {
            const f = FUNCS[state.funcKey];
            return { xMin: f.rangeX[0], xMax: f.rangeX[1], yMin: f.rangeY[0], yMax: f.rangeY[1] };
        }

        function toCanvasX(mathX) {
            const { xMin, xMax } = getRange();
            return MARGIN.left + ((mathX - xMin) / (xMax - xMin)) * (W - MARGIN.left - MARGIN.right);
        }

        function toCanvasY(mathY) {
            const { yMin, yMax } = getRange();
            return MARGIN.top + ((yMax - mathY) / (yMax - yMin)) * (H - MARGIN.top - MARGIN.bottom);
        }

        function toMathX(canvasX) {
            const { xMin, xMax } = getRange();
            return xMin + ((canvasX - MARGIN.left) / (W - MARGIN.left - MARGIN.right)) * (xMax - xMin);
        }

        function toMathY(canvasY) {
            const { yMin, yMax } = getRange();
            return yMax - ((canvasY - MARGIN.top) / (H - MARGIN.top - MARGIN.bottom)) * (yMax - yMin);
        }

        // ============================================================
        // 5. Drawing core
        // ============================================================
        function draw() {
            const f = FUNCS[state.funcKey];
            const { xMin, xMax, yMin, yMax } = getRange();
            const x0 = state.x0;
            const h = state.h;

            ctx.clearRect(0, 0, W, H);

            // Background
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, W, H);

            // Grid
            ctx.save();
            ctx.strokeStyle = '#eef2f7';
            ctx.lineWidth = 1;
            const xStep = (xMax - xMin) / 10;
            for (let x = Math.ceil(xMin / xStep) * xStep; x <= xMax; x += xStep) {
                if (Math.abs(x) < 1e-10) continue;
                const cx = toCanvasX(x);
                ctx.beginPath();
                ctx.moveTo(cx, MARGIN.top);
                ctx.lineTo(cx, H - MARGIN.bottom);
                ctx.stroke();
            }
            const yStep = (yMax - yMin) / 8;
            for (let y = Math.ceil(yMin / yStep) * yStep; y <= yMax; y += yStep) {
                if (Math.abs(y) < 1e-10) continue;
                const cy = toCanvasY(y);
                ctx.beginPath();
                ctx.moveTo(MARGIN.left, cy);
                ctx.lineTo(W - MARGIN.right, cy);
                ctx.stroke();
            }
            ctx.restore();

            // Axes
            ctx.save();
            ctx.strokeStyle = '#3a4d62';
            ctx.lineWidth = 1.8;
            const y0 = toCanvasY(0);
            ctx.beginPath();
            ctx.moveTo(MARGIN.left, y0);
            ctx.lineTo(W - MARGIN.right + 10, y0);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(W - MARGIN.right + 10, y0);
            ctx.lineTo(W - MARGIN.right + 2, y0 - 6);
            ctx.lineTo(W - MARGIN.right + 2, y0 + 6);
            ctx.closePath();
            ctx.fillStyle = '#3a4d62';
            ctx.fill();
            ctx.stroke();

            const x0_canvas = toCanvasX(0);
            ctx.beginPath();
            ctx.moveTo(x0_canvas, MARGIN.top - 10);
            ctx.lineTo(x0_canvas, H - MARGIN.bottom);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(x0_canvas, MARGIN.top - 10);
            ctx.lineTo(x0_canvas - 6, MARGIN.top - 2);
            ctx.lineTo(x0_canvas + 6, MARGIN.top - 2);
            ctx.closePath();
            ctx.fillStyle = '#3a4d62';
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#2d4055';
            ctx.font = '15px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'top';
            ctx.fillText('x', W - MARGIN.right + 16, y0 + 6);
            ctx.textAlign = 'center';
            ctx.textBaseline = 'bottom';
            ctx.fillText('y', x0_canvas + 12, MARGIN.top - 8);
            ctx.restore();

            // Tick labels
            ctx.save();
            ctx.fillStyle = '#4a5b6e';
            ctx.font = '12px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'top';
            for (let x = Math.ceil(xMin / xStep) * xStep; x <= xMax; x += xStep) {
                if (Math.abs(x) < 1e-8) continue;
                const cx = toCanvasX(x);
                ctx.fillText(Number.isInteger(x) ? x.toString() : x.toFixed(1), cx, y0 + 4);
            }
            ctx.textAlign = 'right';
            ctx.textBaseline = 'middle';
            for (let y = Math.ceil(yMin / yStep) * yStep; y <= yMax; y += yStep) {
                if (Math.abs(y) < 1e-8) continue;
                const cy = toCanvasY(y);
                ctx.fillText(Number.isInteger(y) ? y.toString() : y.toFixed(1), x0_canvas - 8, cy);
            }
            ctx.textAlign = 'right';
            ctx.textBaseline = 'top';
            ctx.fillText('O', x0_canvas - 8, y0 + 4);
            ctx.restore();

            // Function curve
            ctx.save();
            ctx.strokeStyle = f.color;
            ctx.lineWidth = 3.0;
            ctx.shadowColor = 'rgba(0,0,0,0.04)';
            ctx.shadowBlur = 6;
            ctx.beginPath();
            const steps = 400;
            let first = true;
            for (let i = 0; i <= steps; i++) {
                const t = i / steps;
                const x = xMin + t * (xMax - xMin);
                const y = f.f(x);
                if (!isFinite(y)) continue;
                const cx = toCanvasX(x);
                const cy = toCanvasY(y);
                if (cy < MARGIN.top - 20 || cy > H - MARGIN.bottom + 20) {
                    first = true;
                    continue;
                }
                if (first) { ctx.moveTo(cx, cy);
                    first = false; } else ctx.lineTo(cx, cy);
            }
            ctx.stroke();
            ctx.shadowBlur = 0;
            ctx.restore();

            // Function label
            ctx.save();
            ctx.fillStyle = f.color;
            ctx.font = 'bold 16px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'left';
            ctx.textBaseline = 'bottom';
            const labelX = toCanvasX((xMin + xMax) * 0.78);
            const labelY = toCanvasY(f.f((xMin + xMax) * 0.78) * 0.88);
            ctx.fillText(`y = ${f.label}`, labelX, labelY);
            ctx.restore();

            // Tangent & secant points
            const x1 = x0;
            const x2 = x0 + h;
            const y1 = f.f(x1);
            const y2 = f.f(x2);

            const mTangent = f.df(x1);
            const mSecant = f.secantSlope(x0, h);

            // Tangent line (red dashed)
            ctx.save();
            const tanX1 = x1 - 1.2;
            const tanX2 = x1 + 1.2;
            const tanY1 = y1 + mTangent * (tanX1 - x1);
            const tanY2 = y1 + mTangent * (tanX2 - x1);
            ctx.strokeStyle = '#c0392b';
            ctx.lineWidth = 2.8;
            ctx.setLineDash([8, 6]);
            ctx.beginPath();
            ctx.moveTo(toCanvasX(tanX1), toCanvasY(tanY1));
            ctx.lineTo(toCanvasX(tanX2), toCanvasY(tanY2));
            ctx.stroke();
            ctx.setLineDash([]);
            ctx.restore();

            // Secant line (blue solid)
            ctx.save();
            ctx.strokeStyle = '#2471a3';
            ctx.lineWidth = 3.0;
            ctx.shadowColor = 'rgba(36,113,163,0.15)';
            ctx.shadowBlur = 8;
            ctx.beginPath();
            ctx.moveTo(toCanvasX(x1), toCanvasY(y1));
            ctx.lineTo(toCanvasX(x2), toCanvasY(y2));
            ctx.stroke();
            ctx.shadowBlur = 0;
            ctx.restore();

            // Point 1 (tangent point) - red
            ctx.save();
            const cx1 = toCanvasX(x1);
            const cy1 = toCanvasY(y1);
            const grad = ctx.createRadialGradient(cx1, cy1, 2, cx1, cy1, 18);
            grad.addColorStop(0, 'rgba(192,57,43,0.25)');
            grad.addColorStop(1, 'rgba(192,57,43,0)');
            ctx.fillStyle = grad;
            ctx.beginPath();
            ctx.arc(cx1, cy1, 18, 0, 2 * Math.PI);
            ctx.fill();
            ctx.fillStyle = '#c0392b';
            ctx.shadowColor = 'rgba(192,57,43,0.3)';
            ctx.shadowBlur = 10;
            ctx.beginPath();
            ctx.arc(cx1, cy1, 8, 0, 2 * Math.PI);
            ctx.fill();
            ctx.shadowBlur = 0;
            ctx.strokeStyle = 'white';
            ctx.lineWidth = 2;
            ctx.stroke();
            ctx.fillStyle = '#c0392b';
            ctx.font = 'bold 14px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'left';
            ctx.textBaseline = 'bottom';
            ctx.fillText(`P (${x1.toFixed(2)}, ${y1.toFixed(2)})`, cx1 + 12, cy1 - 4);
            ctx.restore();

            // Point 2 (secant point) - blue
            ctx.save();
            const cx2 = toCanvasX(x2);
            const cy2 = toCanvasY(y2);
            const grad2 = ctx.createRadialGradient(cx2, cy2, 2, cx2, cy2, 18);
            grad2.addColorStop(0, 'rgba(36,113,163,0.25)');
            grad2.addColorStop(1, 'rgba(36,113,163,0)');
            ctx.fillStyle = grad2;
            ctx.beginPath();
            ctx.arc(cx2, cy2, 18, 0, 2 * Math.PI);
            ctx.fill();
            ctx.fillStyle = '#2471a3';
            ctx.shadowColor = 'rgba(36,113,163,0.3)';
            ctx.shadowBlur = 10;
            ctx.beginPath();
            ctx.arc(cx2, cy2, 8, 0, 2 * Math.PI);
            ctx.fill();
            ctx.shadowBlur = 0;
            ctx.strokeStyle = 'white';
            ctx.lineWidth = 2;
            ctx.stroke();
            ctx.fillStyle = '#2471a3';
            ctx.font = 'bold 14px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'left';
            ctx.textBaseline = 'bottom';
            ctx.fillText(`Q (${x2.toFixed(2)}, ${y2.toFixed(2)})`, cx2 + 12, cy2 - 4);
            ctx.restore();

            // h label
            ctx.save();
            ctx.fillStyle = '#1a2b3f';
            ctx.font = '15px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'top';
            const midX = (cx1 + cx2) / 2;
            const labelYpos = Math.min(cy1, cy2) - 26;
            ctx.fillText(`h = ${h.toFixed(3)}`, midX, labelYpos);
            ctx.strokeStyle = '#1a2b3f';
            ctx.lineWidth = 1.2;
            ctx.setLineDash([3, 4]);
            ctx.beginPath();
            ctx.moveTo(cx1, labelYpos - 6);
            ctx.lineTo(cx2, labelYpos - 6);
            ctx.stroke();
            ctx.setLineDash([]);
            ctx.restore();

            // Legend
            ctx.save();
            const legX = MARGIN.left + 16;
            const legY = MARGIN.top + 16;
            ctx.fillStyle = 'rgba(255,255,255,0.85)';
            ctx.shadowColor = 'rgba(0,0,0,0.04)';
            ctx.shadowBlur = 12;
            ctx.beginPath();
            ctx.roundRect(legX, legY, 166, 84, 12);
            ctx.fill();
            ctx.shadowBlur = 0;
            ctx.strokeStyle = 'rgba(200,212,225,0.3)';
            ctx.lineWidth = 1;
            ctx.beginPath();
            ctx.roundRect(legX, legY, 166, 84, 12);
            ctx.stroke();

            let lx = legX + 14,
                ly = legY + 14;
            ctx.font = '13px "Segoe UI", system-ui, sans-serif';
            ctx.textAlign = 'left';
            ctx.textBaseline = 'middle';
            // Tangent
            ctx.strokeStyle = '#c0392b';
            ctx.lineWidth = 2.8;
            ctx.setLineDash([8, 6]);
            ctx.beginPath();
            ctx.moveTo(lx, ly + 4);
            ctx.lineTo(lx + 22, ly + 4);
            ctx.stroke();
            ctx.setLineDash([]);
            ctx.fillStyle = '#2d4055';
            ctx.textBaseline = 'middle';
            ctx.fillText('Tangent', lx + 30, ly + 4);
            // Secant
            ly += 28;
            ctx.strokeStyle = '#2471a3';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(lx, ly + 4);
            ctx.lineTo(lx + 22, ly + 4);
            ctx.stroke();
            ctx.fillStyle = '#2d4055';
            ctx.fillText('Secant', lx + 30, ly + 4);
            // Curve
            ly += 28;
            ctx.strokeStyle = f.color;
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(lx, ly + 4);
            ctx.lineTo(lx + 22, ly + 4);
            ctx.stroke();
            ctx.fillStyle = '#2d4055';
            ctx.fillText(`y = ${f.label}`, lx + 30, ly + 4);
            ctx.restore();

            // Update stats
            const diff = Math.abs(mSecant - mTangent);
            const pct = Math.abs(mTangent) > 1e-12 ? (diff / Math.abs(mTangent)) * 100 : (diff * 100);
            tangentSlopeEl.textContent = mTangent.toFixed(5);
            secantSlopeEl.textContent = mSecant.toFixed(5);
            diffValueEl.textContent = diff.toFixed(5);
            diffPercentEl.textContent = pct.toFixed(1) + '%';
            hStatEl.textContent = h.toFixed(4);

            if (diff < 0.01 && h < 0.05) {
                diffValueEl.className = 'num diff-close';
                diffPercentEl.style.background = '#27ae60';
                diffPercentEl.style.color = 'white';
            } else {
                diffValueEl.className = 'num diff';
                diffPercentEl.style.background = '#e9eef4';
                diffPercentEl.style.color = '#1a2b3f';
            }

            hDisplay.textContent = h.toFixed(3);
            x0Display.textContent = x0.toFixed(2);
        }

        if (!CanvasRenderingContext2D.prototype.roundRect) {
            CanvasRenderingContext2D.prototype.roundRect = function(x, y, w, h, r) {
                if (r > w / 2) r = w / 2;
                if (r > h / 2) r = h / 2;
                this.moveTo(x + r, y);
                this.arcTo(x + w, y, x + w, y + h, r);
                this.arcTo(x + w, y + h, x, y + h, r);
                this.arcTo(x, y + h, x, y, r);
                this.arcTo(x, y, x + w, y, r);
                return this;
            };
        }

        // ============================================================
        // 6. Interaction
        // ============================================================
        function updateFromSliders() {
            const raw = parseFloat(hSlider.value);
            const h = Math.pow(10, raw);
            state.h = Math.min(2.0, Math.max(0.005, h));
            state.x0 = parseFloat(x0Slider.value);
            draw();
        }

        hSlider.addEventListener('input', updateFromSliders);
        x0Slider.addEventListener('input', updateFromSliders);

        funcSelect.addEventListener('change', () => {
            state.funcKey = funcSelect.value;
            const f = FUNCS[state.funcKey];
            let newX0 = state.x0;
            if (state.x0 < f.rangeX[0] + 0.1) newX0 = f.rangeX[0] + 0.3;
            if (state.x0 > f.rangeX[1] - 0.1) newX0 = f.rangeX[1] - 0.3;
            state.x0 = newX0;
            x0Slider.value = newX0;
            x0Display.textContent = newX0.toFixed(2);
            draw();
        });

        resetBtn.addEventListener('click', () => {
            if (state.autoMode) stopAuto();
            state.x0 = 1.0;
            state.h = 0.80;
            x0Slider.value = '1.0';
            hSlider.value = '-0.3';
            x0Display.textContent = '1.00';
            hDisplay.textContent = '0.800';
            funcSelect.value = 'square';
            state.funcKey = 'square';
            draw();
        });

        // ============================================================
        // 7. Auto Demo
        // ============================================================
        let autoRunning = false;

        function stopAuto() {
            if (state.autoTimer) {
                clearInterval(state.autoTimer);
                state.autoTimer = null;
            }
            autoRunning = false;
            autoBtn.textContent = '▶ Auto Demo';
            autoBtn.classList.remove('active');
        }

        function startAuto() {
            if (autoRunning) return;
            autoRunning = true;
            autoBtn.textContent = '⏹ Stop';
            autoBtn.classList.add('active');

            let hVal = 1.8;
            const step = -0.012;

            state.autoTimer = setInterval(() => {
                hVal += step;
                if (hVal < 0.005) {
                    hVal = 0.005;
                    clearInterval(state.autoTimer);
                    state.autoTimer = null;
                    autoRunning = false;
                    autoBtn.textContent = '▶ Auto Demo';
                    autoBtn.classList.remove('active');
                    state.h = hVal;
                    const logVal = Math.log10(hVal);
                    hSlider.value = Math.max(-3, Math.min(0, logVal));
                    hDisplay.textContent = hVal.toFixed(3);
                    draw();
                    setTimeout(() => {
                        if (!autoRunning) {
                            hVal = 1.8;
                            startAuto();
                        }
                    }, 1800);
                    return;
                }
                state.h = hVal;
                const logVal = Math.log10(hVal);
                hSlider.value = Math.max(-3, Math.min(0, logVal));
                hDisplay.textContent = hVal.toFixed(3);
                draw();
            }, 80);
        }

        autoBtn.addEventListener('click', () => {
            if (autoRunning) {
                stopAuto();
            } else {
                startAuto();
            }
        });

        hSlider.addEventListener('input', () => {
            if (autoRunning) stopAuto();
        });
        x0Slider.addEventListener('input', () => {
            if (autoRunning) stopAuto();
        });
        funcSelect.addEventListener('change', () => {
            if (autoRunning) stopAuto();
        });

        // ============================================================
        // 8. Resize & Init
        // ============================================================
        function resizeCanvas() {
            const rect = canvas.getBoundingClientRect();
            const containerWidth = rect.width;
            const aspect = 1100 / 620;
            let displayWidth = containerWidth;
            let displayHeight = containerWidth / aspect;
            const maxHeight = window.innerHeight * 0.55;
            if (displayHeight > maxHeight) {
                displayHeight = maxHeight;
                displayWidth = maxHeight * aspect;
            }
            canvas.style.width = displayWidth + 'px';
            canvas.style.height = displayHeight + 'px';
            W = 1100;
            H = 620;
            canvas.width = W;
            canvas.height = H;
            draw();
        }

        window.addEventListener('resize', resizeCanvas);

        function init() {
            hSlider.value = '-0.3';
            x0Slider.value = '1.0';
            state.h = 0.80;
            state.x0 = 1.0;
            hDisplay.textContent = '0.800';
            x0Display.textContent = '1.00';
            resizeCanvas();
        }

        if (document.readyState === 'complete') {
            init();
        } else {
            window.addEventListener('load', init);
        }

        canvas.addEventListener('mousemove', (e) => {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            const canvasX = (e.clientX - rect.left) * scaleX;
            const canvasY = (e.clientY - rect.top) * scaleY;
            if (canvasX < MARGIN.left || canvasX > W - MARGIN.right ||
                canvasY < MARGIN.top || canvasY > H - MARGIN.bottom) {
                canvas.style.cursor = 'default';
                return;
            }
            canvas.style.cursor = 'crosshair';
        });

        canvas.addEventListener('mouseleave', () => {
            canvas.style.cursor = 'default';
        });

        console.log('✅ Interactive math demo loaded!');
        console.log('💡 Drag sliders to adjust h and x₀, watch the secant slope approach the tangent slope.');
    </script>
</body>
</html>
