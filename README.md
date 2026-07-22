# douyin<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>抹山集 · 盲盒活动数据复盘</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;scroll-behavior:smooth}
:root{
  --primary:#1a1a2e;
  --accent:#e94560;
  --accent2:#0f3460;
  --gold:#f5c518;
  --card-bg:#fff;
  --bg:#f8f6f2;
  --text:#222;
  --text2:#555;
  --border:#eee;
  --shadow:0 4px 24px rgba(0,0,0,0.06);
  --radius:12px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI','PingFang SC','Microsoft YaHei',sans-serif;background:var(--bg);color:var(--text);line-height:1.6}
a{color:var(--accent);text-decoration:none}

/* Nav */
nav{position:fixed;top:0;left:0;right:0;z-index:1000;background:rgba(26,26,46,0.92);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);padding:0 24px;display:flex;align-items:center;height:56px;gap:4px;overflow-x:auto;white-space:nowrap}
nav a{color:rgba(255,255,255,0.7);padding:6px 14px;border-radius:8px;font-size:13px;font-weight:500;transition:.25s;flex-shrink:0}
nav a:hover,nav a.active{color:#fff;background:rgba(233,69,96,0.25)}
.nav-brand{color:#fff;font-weight:700;font-size:15px;margin-right:12px;flex-shrink:0}

/* Sections */
.section{padding:80px 24px 60px;max-width:1280px;margin:0 auto}
.section-header{text-align:center;margin-bottom:40px}
.section-header h2{font-size:28px;font-weight:700;color:var(--primary);margin-bottom:8px}
.section-header p{color:var(--text2);font-size:15px}
.tag{display:inline-block;background:var(--accent);color:#fff;padding:4px 16px;border-radius:20px;font-size:12px;font-weight:600;margin-bottom:12px}

/* Hero */
.hero{background:linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);color:#fff;padding:120px 24px 80px;text-align:center;position:relative;overflow:hidden}
.hero::before{content:'';position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:radial-gradient(circle at 30% 50%, rgba(233,69,96,0.12) 0%, transparent 50%),radial-gradient(circle at 70% 50%, rgba(245,197,24,0.08) 0%, transparent 50%);animation:heroGlow 8s ease-in-out infinite alternate}
@keyframes heroGlow{0%{transform:translate(0,0)}100%{transform:translate(-10%,-10%)}}
.hero>*{position:relative;z-index:1}
.hero h1{font-size:42px;font-weight:800;margin-bottom:12px;letter-spacing:-0.5px}
.hero p{font-size:16px;opacity:0.8;margin-bottom:40px;max-width:600px;margin-left:auto;margin-right:auto}

/* Counter Grid */
.counter-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:16px;max-width:900px;margin:0 auto}
.counter-card{background:rgba(255,255,255,0.08);backdrop-filter:blur(8px);border-radius:var(--radius);padding:20px 16px;text-align:center;border:1px solid rgba(255,255,255,0.1);transition:.3s}
.counter-card:hover{background:rgba(255,255,255,0.14);transform:translateY(-2px)}
.counter-card .num{font-size:28px;font-weight:700;color:var(--gold);margin:4px 0}
.counter-card .label{font-size:12px;opacity:0.7}

/* Cards */
.card{border-radius:var(--radius);background:var(--card-bg);box-shadow:var(--shadow);padding:24px;margin-bottom:24px;transition:.3s}
.card:hover{box-shadow:0 8px 32px rgba(0,0,0,0.08)}
.card h3{font-size:17px;font-weight:600;margin-bottom:16px;color:var(--primary);display:flex;align-items:center;gap:8px}
.card h3 .badge{font-size:11px;background:var(--accent);color:#fff;padding:2px 10px;border-radius:12px;font-weight:500}

/* Grid */
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:24px}
.grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:20px}
@media(max-width:860px){.grid-2,.grid-3{grid-template-columns:1fr}}

/* Metric Box */
.metric-row{display:flex;flex-wrap:wrap;gap:12px;margin-bottom:16px}
.metric{flex:1;min-width:100px;padding:12px 14px;background:#f8f6f2;border-radius:8px;text-align:center}
.metric .val{font-size:20px;font-weight:700;color:var(--primary)}
.metric .lbl{font-size:11px;color:var(--text2);margin-top:2px}
.metric .chg{font-size:11px;font-weight:600;margin-top:1px}
.chg.up{color:#22c55e}
.chg.down{color:#ef4444}

/* Chart Container */
.chart-wrap{position:relative;width:100%;max-height:400px}
.chart-wrap canvas{width:100%!important;max-height:380px}
.chart-container{position:relative}
.expand-btn{position:absolute;top:8px;right:8px;background:rgba(0,0,0,0.06);border:none;border-radius:6px;padding:4px 10px;font-size:11px;cursor:pointer;color:var(--text2);z-index:5;transition:.2s}
.expand-btn:hover{background:rgba(0,0,0,0.12)}

/* Modal */
.modal{display:none;position:fixed;top:0;left:0;right:0;bottom:0;z-index:2000;background:rgba(0,0,0,0.6);backdrop-filter:blur(4px);justify-content:center;align-items:center;padding:24px}
.modal.open{display:flex}
.modal-content{background:#fff;border-radius:var(--radius);width:90vw;max-width:1100px;max-height:85vh;padding:32px;position:relative;overflow:auto;box-shadow:0 24px 64px rgba(0,0,0,0.2)}
.modal-close{position:sticky;top:0;float:right;background:var(--accent);color:#fff;border:none;width:32px;height:32px;border-radius:50%;font-size:18px;cursor:pointer;z-index:10;display:flex;align-items:center;justify-content:center;transition:.2s}
.modal-close:hover{transform:scale(1.1)}
.modal-content h3{margin-bottom:20px}

/* Summary */
.summary-block{padding:20px;background:linear-gradient(135deg,#fef3c7,#fde68a);border-radius:var(--radius);margin-bottom:20px;border-left:4px solid var(--gold)}
.summary-block h4{color:var(--primary);margin-bottom:8px;font-size:15px}
.summary-block p{font-size:14px;color:#444;line-height:1.7}

/* Animations */
@keyframes fadeUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
.fade-up{animation:fadeUp 0.6s ease both}
.delay-1{animation-delay:0.1s}
.delay-2{animation-delay:0.2s}
.delay-3{animation-delay:0.3s}
.delay-4{animation-delay:0.4s}

/* Table */
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{padding:10px 12px;text-align:center;border-bottom:1px solid var(--border)}
th{background:var(--primary);color:#fff;font-weight:600;font-size:12px}
tr:nth-child(even){background:#fafafa}

/* Color dots */
.dot{display:inline-block;width:10px;height:10px;border-radius:50%;margin-right:6px;vertical-align:middle}

/* Period tags */
.period-pre{background:#e0e7ff;color:#3730a3;padding:2px 10px;border-radius:12px;font-size:11px;font-weight:600}
.period-dur{background:#fce7f3;color:#be185d;padding:2px 10px;border-radius:12px;font-size:11px;font-weight:600}
.period-post{background:#d1fae5;color:#047857;padding:2px 10px;border-radius:12px;font-size:11px;font-weight:600}

.footer{text-align:center;padding:32px 24px;color:var(--text2);font-size:13px;border-top:1px solid var(--border);margin-top:20px}

.chart-toggle{display:flex;gap:6px;margin-bottom:12px;flex-wrap:wrap}
.chart-toggle button{padding:4px 14px;border:1px solid var(--border);border-radius:16px;background:#fff;cursor:pointer;font-size:12px;transition:.2s}
.chart-toggle button.active{background:var(--primary);color:#fff;border-color:var(--primary)}
</style>
</head>
<body>

<nav>
  <span class="nav-brand">🎁 抹山集·盲盒复盘</span>
  <a href="#overview" class="active">总览</a>
  <a href="#old">老店分析</a>
  <a href="#new">新店分析</a>
  <a href="#summary">总结</a>
</nav>

<!-- ==================== HERO ==================== -->
<section class="hero" id="overview">
  <h1>抹山集 · 夏日盲盒活动数据复盘</h1>
  <p>基于老店（青云市集、花果园购物中心）及新店（太平路、福州街、鸿通城）活动期间全量抖音经营数据</p>

  <!-- Big counters -->
  <div class="counter-grid">
    <div class="counter-card fade-up"><div class="label">⭐ 活动新增评价</div><div class="num" data-target="343">0</div><div class="label">总新增评价（5店）</div></div>
    <div class="counter-card fade-up delay-1"><div class="label">🏆 好评率</div><div class="num">99.1%</div><div class="label">好评率</div></div>
    <div class="counter-card fade-up delay-2"><div class="label">📈 老店评分提升</div><div class="num">4.4→4.7</div><div class="label">花果园评分跃升</div></div>
    <div class="counter-card fade-up delay-3"><div class="label">🔥 活动评价爆发</div><div class="num">+780~3750%</div><div class="label">活动期评价增量</div></div>
    <div class="counter-card fade-up delay-4"><div class="label">💯 新店口碑</div><div class="num">100%</div><div class="label">新店好评率全满</div></div>
  </div>
</section>

<!-- ==================== 老店分析 ==================== -->
<section class="section" id="old">
  <div class="section-header fade-up">
    <span class="tag">一、老店分析</span>
    <h2>青云市集 vs 花果园购物中心</h2>
    <p>6月27日-7月3日（无盲盒）→ 7月4日-7月10日（盲盒活动）→ 7月11日-7月18日（活动后）</p>
  </div>

  <!-- Period definition -->
  <div class="card fade-up" style="background:linear-gradient(135deg,#fff7ed,#ffedd5);border-left:4px solid #f97316">
    <h3>🎯 活动目标：提升好评与评分</h3>
    <p style="font-size:14px;color:#444;line-height:1.7">本次盲盒活动的核心目标是<strong>激发顾客主动评价、拉升门店评分</strong>。以下分析围绕「评价数量增长」「好评率」「评分变化」三条主线展开，核销数据作为辅助参考。</p>
    <div style="display:flex;gap:12px;flex-wrap:wrap;margin-top:10px">
      <span><span class="period-pre">活动前</span> 6/27 - 7/3 &nbsp;无盲盒活动</span>
      <span><span class="period-dur">活动中</span> 7/4 - 7/10 &nbsp;盲盒活动进行中</span>
      <span><span class="period-post">活动后</span> 7/11 - 7/18 &nbsp;盲盒活动结束</span>
    </div>
  </div>

  <!-- Trend Chart: PV + Intent Amount -->
  <div class="card fade-up">
    <h3>📈 门店访问量趋势 <span class="badge">日趋势</span></h3>
    <div class="chart-toggle" id="oldTrendToggle">
      <button class="active" data-view="pv">访问次数</button>
      <button data-view="visitor">访问人数</button>
      <button data-view="clk">商品点击</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('oldTrendChart','青云市集 vs 花果园 · 门店访问趋势')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="oldTrendChart"></canvas></div>
    </div>
  </div>

  <!-- Intent + Verify Amount -->
  <div class="card fade-up">
    <h3>💰 意向成交与核销金额趋势 <span class="badge">日趋势</span></h3>
    <div class="chart-toggle" id="oldAmtToggle">
      <button class="active" data-view="intent">意向金额</button>
      <button data-view="verify">核销金额</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('oldAmtChart','意向成交与核销金额 · 日趋势')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="oldAmtChart"></canvas></div>
    </div>
  </div>

  <!-- Rating & Reviews -->
  <div class="card fade-up">
    <h3>⭐ 门店评分与评价趋势 <span class="badge">评价分析</span></h3>
    <div class="chart-toggle" id="oldReviewToggle">
      <button class="active" data-view="review">日新增评价</button>
      <button data-view="rating">评分趋势</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('oldReviewChart','青云市集 vs 花果园 · 评价与评分趋势')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="oldReviewChart"></canvas></div>
    </div>
  </div>

  <!-- Review period comparison -->
  <div class="card fade-up">
    <h3>📊 三期评价对比 <span class="badge">活动前后</span></h3>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('oldReviewPeriodChart','老店·三期评价对比')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="oldReviewPeriodChart"></canvas></div>
    </div>
  </div>


  <!-- 3-Period Comparison -->
  <div class="card fade-up">
    <h3>📊 三期数据对比 <span class="badge">活动前后对比</span></h3>
    <div class="chart-toggle" id="oldPeriodToggle">
      <button class="active" data-view="pv">访问次数</button>
      <button data-view="verifyAmt">核销金额</button>
      <button data-view="verifyCoupon">核销券数</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('oldPeriodChart','老店·三期核心数据对比')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="oldPeriodChart"></canvas></div>
    </div>
  </div>

  <!-- Metric cards -->
  <div class="grid-2 fade-up">
    <div class="card">
      <h3>🏪 青云市集旗舰店 · 评价数据</h3>
      <div class="metric-row">
        <div class="metric"><div class="val">5→44</div><div class="lbl">新增评价（活动期）</div><div class="chg up">+780%</div></div>
        <div class="metric"><div class="val">4.7→4.8</div><div class="lbl">评分变化</div></div>
        <div class="metric"><div class="val">83</div><div class="lbl">总新增评价</div></div>
        <div class="metric"><div class="val">98.8%</div><div class="lbl">好评率</div></div>
      </div>
      <div style="font-size:13px;color:var(--text2)">
        活动期评价从 5 条飙升至 <strong>44 条</strong>（+780%），活动后仍保持 <strong>34 条</strong> 新增，口碑持续发酵<br>
        核销金额活动期 +87.5%，访问量 +73.9%
      </div>
    </div>
    <div class="card">
      <h3>🛍️ 花果园购物中心旗舰店 · 评价数据</h3>
      <div class="metric-row">
        <div class="metric"><div class="val">2→77</div><div class="lbl">新增评价（活动期）</div><div class="chg up">+3750%</div></div>
        <div class="metric"><div class="val">4.4→4.7</div><div class="lbl">评分变化</div></div>
        <div class="metric"><div class="val">106</div><div class="lbl">总新增评价</div></div>
        <div class="metric"><div class="val">96.9%</div><div class="lbl">好评率</div></div>
      </div>
      <div style="font-size:13px;color:var(--text2)">
        活动期评价从 2 条飙升至 <strong>77 条</strong>（+3750%），评分从 4.4 跃升至 <strong>4.7</strong><br>
        核销金额活动期 +34.6%，访问量 +74.4%
      </div>
    </div>
  </div>

  <!-- Detail Table -->
  <div class="card fade-up">
    <h3>📋 老店三期详细数据对比</h3>
    <div style="overflow-x:auto">
    <table>
      <thead>
        <tr><th>门店</th><th>指标</th><th>活动前 (6/27-7/3)</th><th>活动中 (7/4-7/10)</th><th>活动后 (7/11-7/18)</th><th>活动期提升</th></tr>
      </thead>
      <tbody>
        <tr style="background:#fff7ed"><td rowspan="2" style="background:#fff7ed"><strong>⭐ 青云市集</strong></td><td><strong>新增评价</strong></td><td><strong>5</strong></td><td><strong>44</strong></td><td><strong>34</strong></td><td><strong><span class="chg up">+780.0%</span></strong></td></tr>
        <tr style="background:#fff7ed"><td><strong>门店评分</strong></td><td><strong>4.7</strong></td><td><strong>4.7</strong></td><td><strong>4.8</strong></td><td><strong><span class="chg up">↑0.1</span></strong></td></tr>
        <tr><td rowspan="3">青云市集</td><td>访问次数</td><td>1,979</td><td>3,441</td><td>2,721</td><td><span class="chg up">+73.9%</span></td></tr>
        <tr><td>意向金额(元)</td><td>3,768.5</td><td>5,489.9</td><td>7,731.0</td><td><span class="chg up">+45.7%</span></td></tr>
        <tr><td>核销金额(元)</td><td>1,607.6</td><td>3,014.9</td><td>5,260.2</td><td><span class="chg up">+87.5%</span></td></tr>
        <tr style="background:#fffbeb"><td rowspan="2" style="background:#fffbeb"><strong>⭐ 花果园</strong></td><td><strong>新增评价</strong></td><td><strong>2</strong></td><td><strong>77</strong></td><td><strong>27</strong></td><td><strong><span class="chg up">+3750.0%</span></strong></td></tr>
        <tr style="background:#fffbeb"><td><strong>门店评分</strong></td><td><strong>4.4</strong></td><td><strong>4.4~4.6</strong></td><td><strong>4.7</strong></td><td><strong><span class="chg up">↑0.3</span></strong></td></tr>
        <tr><td rowspan="3">花果园</td><td>访问次数</td><td>1,189</td><td>2,074</td><td>1,792</td><td><span class="chg up">+74.4%</span></td></tr>
        <tr><td>意向金额(元)</td><td>3,830.2</td><td>4,794.8</td><td>7,050.1</td><td><span class="chg up">+25.2%</span></td></tr>
        <tr><td>核销金额(元)</td><td>2,639.0</td><td>3,553.0</td><td>5,591.8</td><td><span class="chg up">+34.6%</span></td></tr>
      </tbody>
    </table>
    </div>
  </div>
</section>

<!-- ==================== 新店分析 ==================== -->
<section class="section" id="new">
  <div class="section-header fade-up">
    <span class="tag">二、新店分析</span>
    <h2>7/10 开业三店 · 盲盒引爆口碑</h2>
    <p>太平路店 · 福州街店 · 鸿通城店 &nbsp;|&nbsp; 7月10日-7月19日 &nbsp;|&nbsp; 重点看评价积累与评分成长</p>
  </div>

  <!-- Single store trends -->
  <div class="card fade-up">
    <h3>📈 三新店访问量趋势</h3>
    <div class="chart-toggle" id="newTrendToggle">
      <button class="active" data-view="pv">访问次数</button>
      <button data-view="verify">核销金额</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('newTrendChart','三新店趋势对比')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="newTrendChart"></canvas></div>
    </div>
  </div>

  <div class="card fade-up">
    <h3>📈 三新店核销券数 &amp; 意向成交券数趋势</h3>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('newCouponChart','三新店券数趋势')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="newCouponChart"></canvas></div>
    </div>
  </div>

  <!-- Store comparison -->
  <div class="card fade-up" style="background:linear-gradient(135deg,#f0fdf4,#dcfce7);border-left:4px solid #22c55e">
    <h3>🏆 新店评价表现 · 核心数据</h3>
    <div class="grid-3">
      <div class="metric" style="background:#dcfce7"><div class="val">27</div><div class="lbl">太平路 · 新增评价</div><div class="chg up">好评率100%</div></div>
      <div class="metric" style="background:#dcfce7"><div class="val">77</div><div class="lbl">福州街 · 新增评价</div><div class="chg up">好评率100%</div></div>
      <div class="metric" style="background:#dcfce7"><div class="val">50</div><div class="lbl">鸿通城 · 新增评价</div><div class="chg up">好评率100%</div></div>
    </div>
    <div class="grid-3">
      <div class="metric"><div class="val">4.3</div><div class="lbl">太平路 · 评分</div></div>
      <div class="metric"><div class="val">4.5→4.7</div><div class="lbl">福州街 · 评分提升</div></div>
      <div class="metric"><div class="val">4.4→4.7</div><div class="lbl">鸿通城 · 评分提升</div></div>
    </div>
    <div style="text-align:center;margin-top:10px;font-size:13px;color:var(--text2)">
      🏅 三店合计 <strong>154 条</strong> 评价，全部好评，无一条中差评！<br>
      福州街上榜「贵阳市饮品店热销榜」，鸿通城同样上榜
    </div>
  </div>

  <!-- Radar comparison -->
  <div class="card fade-up">
    <h3>⚡ 新店综合能力对比雷达图</h3>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('newRadarChart','新店综合能力对比')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="newRadarChart"></canvas></div>
    </div>
  </div>

  <!-- New store reviews trend -->
  <div class="card fade-up">
    <h3>⭐ 新店评价趋势 <span class="badge">评价分析</span></h3>
    <div class="chart-toggle" id="newReviewToggle">
      <button class="active" data-view="daily">日新增评价</button>
      <button data-view="cumulative">累计评价</button>
    </div>
    <div class="chart-container">
      <button class="expand-btn" onclick="expandChart('newReviewChart','三新店评价累积趋势')">⛶ 放大</button>
      <div class="chart-wrap"><canvas id="newReviewChart"></canvas></div>
    </div>
  </div>


  <!-- Detail Table -->
  <div class="card fade-up">
    <h3>📋 新店详细数据</h3>
    <div style="overflow-x:auto">
    <table>
      <thead>
        <tr><th>门店</th><th>⭐ 新增评价</th><th>⭐ 好评率</th><th>⭐ 评分</th><th>评分成长</th><th>上榜情况</th><th>访问次数</th><th>核销金额</th></tr>
      </thead>
      <tbody>
        <tr><td><strong>太平路店</strong></td><td><strong>27</strong></td><td><strong>100%</strong></td><td><strong>4.3</strong></td><td>0→4.3</td><td>-</td><td>2,481</td><td>¥4,922</td></tr>
        <tr><td><strong>福州街店</strong></td><td><strong>77</strong></td><td><strong>100%</strong></td><td><strong>4.7</strong></td><td>4.5→4.7</td><td><span class="chg up">贵阳市热销榜</span></td><td>2,494</td><td>¥2,189</td></tr>
        <tr><td><strong>鸿通城店</strong></td><td><strong>50</strong></td><td><strong>100%</strong></td><td><strong>4.7</strong></td><td>4.4→4.7</td><td><span class="chg up">贵阳市热销榜</span></td><td>767</td><td>¥1,542</td></tr>
        <tr style="background:#f0f0f0;font-weight:600"><td><strong>合计</strong></td><td><strong>154</strong></td><td><strong>100%</strong></td><td><strong>—</strong></td><td><strong>—</strong></td><td><strong>2店上榜</strong></td><td><strong>5,742</strong></td><td><strong>¥8,652</strong></td></tr>
      </tbody>
    </table>
    </div>
  </div>



</section>

<!-- ==================== 总结 ==================== -->
<section class="section" id="summary">
  <div class="section-header fade-up">
    <span class="tag">三、活动总结</span>
    <h2>盲盒活动复盘结论</h2>
  </div>

  <div class="grid-2">
    <div class="card fade-up">
      <h3>🎯 核心发现</h3>
      <div class="summary-block">
        <h4>1. 老店：盲盒活动显著提升到店转化</h4>
        <p>青云市集和花果园两家老店在盲盒活动期间，门店访问量较活动前分别提升 <strong>73.9%</strong> 和 <strong>74.4%</strong>，核销金额提升 <strong>87.5%</strong> 和 <strong>34.6%</strong>。活动对青云市集的核销转化拉动尤为明显。</p>
      </div>
      <div class="summary-block">
        <h4>2. 活动后效应：核销反超意向</h4>
        <p>活动结束后一周（7/11-7/18），青云市集核销金额达 ¥5,260，花果园达 ¥5,592，均超过活动期。说明盲盒活动带来的品牌曝光和口碑积累具有<strong>持续释放</strong>效应，消费者到店核销存在延迟转化。</p>
      </div>
      <div class="summary-block">
        <h4>3. 花果园：高客单价优势</h4>
        <p>活动引爆口碑：评价量暴增</h4>
        <p>青云市集活动中新增 <strong>44 条</strong> 评价（好评率100%），较活动前增长 <strong>7.8 倍</strong>；花果园新增 <strong>77 条</strong> 评价，增长 <strong>37.5 倍</strong>。活动后青云仍保持 <strong>34 条</strong> 新增好评。盲盒活动在<strong>撬动消费者主动评价</strong>方面效果极为显著。</p>
        <p>花果园评分从活动前的 <strong>4.4</strong> 提升至活动后的 <strong>4.7</strong>，青云从 <strong>4.7</strong> 提升至 <strong>4.8</strong>，口碑质量持续走高。</p>
      </div>
      <div class="summary-block">
        <h4>花果园各期核销金额均高于青云，活动后核销金额 ¥5,592（核销券数 511，客单价 ¥10.94），而青云活动后核销金额 ¥5,260（核销券数 519，客单价 ¥10.14），花果园客单价略优。</p>
      </div>
    </div>

    <div class="card fade-up">
      <h3>💡 新店洞察</h3>
      <div class="summary-block" style="border-left-color:#22c55e;background:#f0fdf4">
        <h4>1. 盲盒引爆口碑：三店 100% 好评开局 🏆</h4>
        <p>三家新店在开业 10 天内累计获得 <strong>154 条</strong> 评价，全部好评，无一条中差评。福州街（77 条）、鸿通城（50 条）连续上榜 <strong>「贵阳市饮品店热销榜」</strong>。盲盒活动成功让新店在开业初期就建立了扎实的口碑基础。</p>
      </div>
      <div class="summary-block">
        <h4>2. 福州街：口碑与流量双丰收</h4>
        <p>福州街店以 <strong>77 条</strong> 评价位居三店之首，评分从开业 4.5 一路攀升至 <strong>4.7</strong>，且持续多日入围热销榜。7月18-19日访问量飙升至 <strong>490/479</strong>，验证了好评积累→曝光增长→更多转化的良性循环。</p>
      </div>
      <div class="summary-block">
        <h4>3. 鸿通城：低起点高成长</h4>
        <p>鸿通城日均访问仅 77 次，但评分从 0 一路飙升至 <strong>4.7</strong>，50 条评价全部 5 星，并登上热销榜。建议后续配合达人探店补齐流量短板，释放口碑优势。</p>
      </div>
      <div class="summary-block">
        <h4>4. 活动带动自然增长</h4>
        <p>三家新店实现 154 条 100% 好评、三店均上榜、<strong>无大额优惠券补贴</strong>。盲盒的<strong>趣味性和社交传播</strong>有效驱动了自然评价和到店转化。</p>
      </div>
    </div>
  </div>

  <div class="card fade-up" style="background:linear-gradient(135deg,#1a1a2e,#16213e);color:#fff;border:none;margin-top:12px">
    <h3 style="color:var(--gold)">📌 最终复盘结论</h3>
    <div style="font-size:14px;line-height:1.8;opacity:0.92">
      <p><strong>1.</strong> 🎯 <strong>首要目标达成：</strong>盲盒活动成功实现了提升好评数量和评分的核心目标。老店活动期新增评价 <strong>121 条</strong>，花果园评分跃升 0.3 分至 4.7；新店累计 <strong>154 条 100% 好评</strong>。</p>
      <p><strong>2.</strong> 📈 <strong>口碑撬动效果：</strong>盲盒的趣味性和社交属性对激发消费者主动评价有极强驱动，低评分门店（花果园）的弹性最高。建议将低评分门店作为未来同类活动的优先投放对象。</p>
      <p><strong>3.</strong> 🚀 <strong>新店冷启动验证有效：</strong>三家新店开业 10 天累计 154 条 100% 好评，福州街、鸿通城双双上榜热销榜，盲盒活动为新店提供了极佳的口碑冷启动场景。</p>
      <p><strong>4.</strong> ⏱ <strong>持续效应明显：</strong>活动结束后两店仍新增 61 条评价，核销金额持续走高。建议设置「盲盒返场」等二次触达机制，充分释放延迟口碑+核销潜力。</p>
      <p><strong>5.</strong> 💡 <strong>后续建议：</strong>鸿通城可配合本地达人探店补足流量；青云作为核销弹性最高的门店可作为未来活动优先投放点；花果园已有评分跃升的基础，可考虑定向好评激励巩固口碑。</p>
    </div>
  </div>
</section>

<div class="footer">抹山集 · 盲盒活动数据复盘报告 · 数据来源：抖音来客 2026年7月</div>

<script>
// ====== DATA ======
const allDates = ['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18'];
const fullDates = ['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18'];

const qyPV = [319,393,96,327,268,239,337,347,342,370,633,880,341,528,565,406,315,322,236,232,275,370];
const qyUV = [249,282,73,225,206,162,232,252,247,261,498,738,263,431,475,346,234,251,189,180,206,274];
const qyClk = [263,420,85,290,202,233,358,275,273,335,296,384,269,267,329,301,182,273,203,182,205,334];
const qyClkU = [114,152,38,116,106,90,122,122,134,148,135,174,129,117,173,149,95,124,95,90,104,142];
const qyIntentAmt = [675.8,513.8,169.7,503.2,461.5,601.7,842.8,957.2,697.1,814,858.9,807.4,509.6,845.7,1314.76,1105.3,608.3,1089,833.6,625.9,986.9,1167.2];
const qyVerifyAmt = [396.86,138.88,100.6,140.96,206.6,187.3,436.4,607.3,209.4,401.7,436.48,499.1,415.6,445.3,1037.35,663.43,503.94,962.28,665.3,391,512.9,523.97];
const qyVerifyC = [56,21,9,14,20,22,45,60,23,41,43,47,36,49,109,63,46,98,65,36,52,50];

const hgyPV = [245,201,68,137,217,134,187,169,250,170,386,638,233,228,271,313,267,219,158,181,179,204];
const hgyUV = [176,146,58,97,187,102,141,133,183,132,312,563,179,166,213,244,205,168,119,147,129,163];
const hgyClk = [198,196,47,143,184,157,190,147,262,144,217,189,154,190,207,215,217,218,148,205,136,180];
const hgyClkU = [103,77,30,61,93,64,79,76,121,79,99,110,83,89,99,114,109,82,71,94,65,88];
const hgyIntentAmt = [831.7,864.1,112.7,397.5,577.6,468.1,578.5,599.6,1005.4,475.4,813.7,380.9,745.3,774.53,1270,827.2,587.6,868.7,787.4,920.8,855.2,933.2];
const hgyVerifyAmt = [681.36,611.9,118.6,287,341.56,137.9,460.66,542.3,707.1,403.9,640.04,306.3,514.2,439.19,896.3,730.3,548.7,706.3,540.1,744.6,674.38,751.1];
const hgyVerifyC = [67,53,13,37,29,15,42,47,74,44,70,30,40,49,84,74,51,64,49,67,50,72];

// New store dates: 7/10 to 7/19
const newDates = ['7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18','7/19'];

const tpPV = [142,279,296,178,122,97,131,179,533,524];
const tpUV = [119,204,222,124,93,69,94,121,390,387];
const tpClk = [175,281,271,184,134,98,99,168,405,341];
const tpClkU = [85,125,125,70,50,42,52,67,144,149];
const tpIntentAmt = [1151.17,1945.8,1631.8,913.1,282.5,263.8,374.2,1178.5,1263.2,1820.7];
const tpIntentC = [65,126,106,69,25,21,34,81,116,140];
const tpVerifyAmt = [518.6,967.5,1039.91,329.4,216.9,187.3,221.5,429.8,169.9,840.9];
const tpVerifyC = [36,67,65,25,18,16,21,44,17,79];

const fzPV = [143,232,268,179,138,131,63,371,490,479];
const fzUV = [97,166,185,119,100,92,44,309,401,386];
const fzClk = [128,229,209,169,103,93,32,98,208,239];
const fzClkU = [63,80,98,75,43,48,13,65,108,121];
const fzIntentAmt = [1432.9,1303.24,851.6,572.4,355.7,219.3,100.7,152.4,702.1,846.6];
const fzIntentC = [65,68,60,48,29,22,9,17,70,58];
const fzVerifyAmt = [482.2,357.6,498.4,290.7,86.4,96.7,8,162.2,99,107.5];
const fzVerifyC = [32,26,35,25,8,11,1,15,13,9];

const htPV = [7,89,138,90,70,77,86,68,65,77];
const htUV = [6,67,98,63,51,52,55,51,51,48];
const htClk = [1,72,130,81,51,75,99,61,45,52];
const htClkU = [1,37,52,38,28,39,34,29,21,25];
const htIntentAmt = [54.6,327.7,436.2,337.5,154,346,372.5,324.6,261.3,257.2];
const htIntentC = [2,23,33,32,15,32,32,29,25,16];
const htVerifyAmt = [0,143.7,257.7,196.1,59.9,255.2,269.5,152.6,64.8,142.6];
const htVerifyC = [0,11,17,18,6,22,23,13,7,12];

// ====== CHART INIT ======
let chartInstances = {};

function createOrUpdate(id, config) {
  if(chartInstances[id]) { chartInstances[id].destroy(); }
  const ctx = document.getElementById(id).getContext('2d');
  chartInstances[id] = new Chart(ctx, config);
}

function expandChart(id, title) {
  document.getElementById('modalTitle').textContent = title;
  const modal = document.getElementById('chartModal');
  modal.classList.add('open');
  // Clone the chart's canvas data into modal
  const srcCanvas = document.getElementById(id);
  const dstCanvas = document.getElementById('modalCanvas');
  // Wait for modal transition
  setTimeout(() => {
    dstCanvas.width = srcCanvas.width * 1.5;
    dstCanvas.height = srcCanvas.height * 1.5;
    // Actually we'll just resize and redraw
    if(chartInstances[id]) {
      if(chartInstances['_modalChart']) chartInstances['_modalChart'].destroy();
      const mCtx = dstCanvas.getContext('2d');
      const cfg = chartInstances[id].config;
      const newCfg = JSON.parse(JSON.stringify(cfg));
      newCfg.options.responsive = true;
      newCfg.options.maintainAspectRatio = true;
      chartInstances['_modalChart'] = new Chart(mCtx, newCfg);
    }
  }, 100);
}

function closeModal() {
  document.getElementById('chartModal').classList.remove('open');
  if(chartInstances['_modalChart']) {
    chartInstances['_modalChart'].destroy();
    chartInstances['_modalChart'] = null;
  }
}

// ====== Chart 1: Old store PV trend ======
function initOldTrend(view) {
  const datasets = [];
  const colors = ['#e94560','#0f3460'];
  const labels = view === 'pv' ? ['青云访问次数','花果园访问次数'] : 
                 view === 'visitor' ? ['青云访问人数','花果园访问人数'] :
                 ['青云商品点击','花果园商品点击'];
  const data = view === 'pv' ? [qyPV, hgyPV] :
               view === 'visitor' ? [qyUV, hgyUV] :
               [qyClk, hgyClk];
  
  // Mark activity period background
  const bgAnnotations = [];
  
  createOrUpdate('oldTrendChart', {
    type: 'line',
    data: {
      labels: fullDates,
      datasets: [
        { label: labels[0], data: data[0], borderColor: colors[0], backgroundColor: colors[0]+'20', fill: true, tension: 0.3, pointRadius: 3, pointHoverRadius: 6 },
        { label: labels[1], data: data[1], borderColor: colors[1], backgroundColor: colors[1]+'20', fill: true, tension: 0.3, pointRadius: 3, pointHoverRadius: 6 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: {
          callbacks: {
            title: function(items) {
              const idx = items[0].dataIndex;
              const dateMap = ['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18'];
              return dateMap[idx] + ' (' + (idx<7?'活动前':idx<14?'活动中':'活动后') + ')';
            }
          }
        }
      },
      scales: {
        x: { grid: { display: false }, ticks: { font: {size:10} } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' } }
      }
    }
  });
}

// ====== Chart 2: Old store AMT trend ======
function initOldAmt(view) {
  const isIntent = view === 'intent';
  const d1 = isIntent ? qyIntentAmt : qyVerifyAmt;
  const d2 = isIntent ? hgyIntentAmt : hgyVerifyAmt;
  const label = isIntent ? '意向金额' : '核销金额';
  
  createOrUpdate('oldAmtChart', {
    type: 'bar',
    data: {
      labels: fullDates,
      datasets: [
        { label: '青云·'+label, data: d1, backgroundColor: 'rgba(233,69,96,0.7)', borderColor: '#e94560', borderWidth: 1, borderRadius: 3 },
        { label: '花果园·'+label, data: d2, backgroundColor: 'rgba(15,52,96,0.7)', borderColor: '#0f3460', borderWidth: 1, borderRadius: 3 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: {
          callbacks: {
            title: function(items) {
              const idx = items[0].dataIndex;
              const dateMap = ['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18'];
              return dateMap[idx] + ' (' + (idx<7?'活动前':idx<14?'活动中':'活动后') + ')';
            },
            label: function(ctx) { return ctx.dataset.label + ': ¥' + ctx.parsed.y.toFixed(2); }
          }
        }
      },
      scales: {
        x: { grid: { display: false }, ticks: { font: {size:10} } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' }, ticks: { callback: v => '¥'+v } }
      }
    }
  });
}

// ====== Chart 3: Period comparison ======
function initOldPeriod(view) {
  // Period data: [Pre, During, Post]
  let qyData, hgyData, label;
  if(view === 'pv') {
    qyData = [1979, 3441, 2721];
    hgyData = [1189, 2074, 1792];
    label = '访问次数';
  } else if(view === 'verifyAmt') {
    qyData = [1607.6, 3014.88, 5260.17];
    hgyData = [2638.98, 3553.03, 5591.78];
    label = '核销金额(¥)';
  } else {
    qyData = [187, 299, 519];
    hgyData = [256, 354, 511];
    label = '核销券数';
  }
  
  createOrUpdate('oldPeriodChart', {
    type: 'bar',
    data: {
      labels: ['活动前 (6/27-7/3)', '盲盒活动中 (7/4-7/10)', '活动后 (7/11-7/18)'],
      datasets: [
        { label: '青云市集', data: qyData, backgroundColor: 'rgba(233,69,96,0.75)', borderRadius: 6 },
        { label: '花果园', data: hgyData, backgroundColor: 'rgba(15,52,96,0.75)', borderRadius: 6 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: {
          callbacks: {
            label: function(ctx) {
              const v = ctx.parsed.y;
              return ctx.dataset.label + ': ' + (view==='verifyAmt'?'¥'+v.toFixed(2):v);
            }
          }
        }
      },
      scales: {
        x: { grid: { display: false } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' } }
      }
    }
  });
}

// ====== Chart 4: New store trend ======
function initNewTrend(view) {
  const isPV = view === 'pv';
  const datasets = isPV ? [
    { label: '太平路', data: tpPV, borderColor: '#e94560', backgroundColor: '#e9456020', fill: true, tension: 0.3, pointRadius: 3 },
    { label: '福州街', data: fzPV, borderColor: '#0f3460', backgroundColor: '#0f346020', fill: true, tension: 0.3, pointRadius: 3 },
    { label: '鸿通城', data: htPV, borderColor: '#f5c518', backgroundColor: '#f5c51820', fill: true, tension: 0.3, pointRadius: 3 }
  ] : [
    { label: '太平路核销', data: tpVerifyAmt, borderColor: '#e94560', backgroundColor: '#e9456020', fill: true, tension: 0.3, pointRadius: 3 },
    { label: '福州街核销', data: fzVerifyAmt, borderColor: '#0f3460', backgroundColor: '#0f346020', fill: true, tension: 0.3, pointRadius: 3 },
    { label: '鸿通城核销', data: htVerifyAmt, borderColor: '#f5c518', backgroundColor: '#f5c51820', fill: true, tension: 0.3, pointRadius: 3 }
  ];
  
  createOrUpdate('newTrendChart', {
    type: 'line',
    data: { labels: newDates, datasets },
    options: {
      responsive: true, maintainAspectRatio: true,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: {
          callbacks: {
            label: function(ctx) {
              return ctx.dataset.label + ': ' + (isPV?ctx.parsed.y:'¥'+ctx.parsed.y.toFixed(2));
            }
          }
        }
      },
      scales: {
        x: { grid: { display: false }, ticks: { font: {size:10} } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' } }
      }
    }
  });
}

// ====== Chart 5: New store coupon trend ======
function initNewCoupon() {
  createOrUpdate('newCouponChart', {
    type: 'line',
    data: {
      labels: newDates,
      datasets: [
        { label: '太平路·核销券数', data: tpVerifyC, borderColor: '#e94560', backgroundColor: '#e9456020', fill: true, tension: 0.3, pointRadius: 3, borderDash: [] },
        { label: '福州街·核销券数', data: fzVerifyC, borderColor: '#0f3460', backgroundColor: '#0f346020', fill: true, tension: 0.3, pointRadius: 3, borderDash: [] },
        { label: '鸿通城·核销券数', data: htVerifyC, borderColor: '#f5c518', backgroundColor: '#f5c51820', fill: true, tension: 0.3, pointRadius: 3, borderDash: [] }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } }
      },
      scales: {
        x: { grid: { display: false }, ticks: { font: {size:10} } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' } }
      }
    }
  });
}

// ====== Chart 6: New store radar ======
function initNewRadar() {
  createOrUpdate('newRadarChart', {
    type: 'radar',
    data: {
      labels: ['访问次数','访问人数','商品点击','意向金额','核销金额','评价数'],
      datasets: [
        { label: '太平路', data: [2481,1823,2156,10825,4922,27], borderColor: '#e94560', backgroundColor: 'rgba(233,69,96,0.15)', pointBackgroundColor: '#e94560', pointRadius: 4 },
        { label: '福州街', data: [2494,1899,1508,6537,2189,77], borderColor: '#0f3460', backgroundColor: 'rgba(15,52,96,0.15)', pointBackgroundColor: '#0f3460', pointRadius: 4 },
        { label: '鸿通城', data: [767,542,667,2872,1542,50], borderColor: '#f5c518', backgroundColor: 'rgba(245,197,24,0.15)', pointBackgroundColor: '#f5c518', pointRadius: 4 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: {
          callbacks: {
            label: function(ctx) {
              const labels = ['访问次数','访问人数','商品点击','意向金额(¥)','核销金额(¥)','评价数'];
              return ctx.dataset.label + ': ' + ctx.parsed.r + (ctx.dataIndex>=3?' 元':'');
            }
          }
        }
      },
      scales: {
        r: { beginAtZero: true, ticks: { display: false, backdropColor: 'transparent' }, grid: { color: '#eee' } }
      }
    }
  });
}


// ====== Chart 7: Old store review & rating ======
function initOldReview() {
  const dailyRevQY = [1,0,1,0,1,1,1,18,1,3,9,9,2,2,2,6,3,12,2,3,5,1];
  const dailyRevHGY = [1,1,0,0,0,0,0,14,25,6,8,1,14,9,17,6,2,0,1,1,0,0];
  const ratingQY = [4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.7,4.8,4.8,4.8,4.8,4.8,4.8];
  const ratingHGY = [4.5,4.5,4.5,4.5,4.4,4.4,4.4,4.4,4.4,4.5,4.5,4.6,4.6,4.6,4.6,4.6,4.7,4.7,4.7,4.7,4.7,4.7];
  
  let mode = 'review';
  this.render = function() {
    if(mode === 'review') {
      createOrUpdate('oldReviewChart', {
        type: 'bar',
        data: {
          labels: fullDates,
          datasets: [
            { label: '青云·新增评价', data: dailyRevQY, backgroundColor: 'rgba(233,69,96,0.7)', borderRadius: 3 },
            { label: '花果园·新增评价', data: dailyRevHGY, backgroundColor: 'rgba(15,52,96,0.7)', borderRadius: 3 }
          ]
        },
        options: {
          responsive: true, maintainAspectRatio: true,
          interaction: { mode: 'index', intersect: false },
          plugins: {
            legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
            tooltip: { callbacks: { title: function(items) { const m=['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18']; return m[items[0].dataIndex] + ' (' + (items[0].dataIndex<7?'活动前':items[0].dataIndex<14?'活动中':'活动后') + ')'; } } }
          },
          scales: { x: { grid: { display: false }, ticks: { font: {size:10} } }, y: { beginAtZero: true, grid: { color: '#f0f0f0' } } }
        }
      });
    } else {
      createOrUpdate('oldReviewChart', {
        type: 'line',
        data: {
          labels: fullDates,
          datasets: [
            { label: '青云·评分', data: ratingQY, borderColor: '#e94560', backgroundColor: '#e9456020', fill: true, tension: 0.2, pointRadius: 4, yAxisID: 'y' },
            { label: '花果园·评分', data: ratingHGY, borderColor: '#0f3460', backgroundColor: '#0f346020', fill: true, tension: 0.2, pointRadius: 4, yAxisID: 'y' }
          ]
        },
        options: {
          responsive: true, maintainAspectRatio: true,
          interaction: { mode: 'index', intersect: false },
          plugins: {
            legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
            tooltip: { callbacks: { title: function(items) { const m=['6/27','6/28','6/29','6/30','7/1','7/2','7/3','7/4','7/5','7/6','7/7','7/8','7/9','7/10','7/11','7/12','7/13','7/14','7/15','7/16','7/17','7/18']; return m[items[0].dataIndex] + ' (' + (items[0].dataIndex<7?'活动前':items[0].dataIndex<14?'活动中':'活动后') + ')'; } } }
          },
          scales: { x: { grid: { display: false }, ticks: { font: {size:10} } }, y: { min: 4.2, max: 5.0, grid: { color: '#f0f0f0' }, ticks: { stepSize: 0.1 } } }
        }
      });
    }
  };
  this.render();
  this.toggle = function(v) { mode = v; this.render(); };
  return this;
}
const _oldReviewCtrl = initOldReview();

// ====== Chart 8: Old store review period comparison ======
function initOldReviewPeriod() {
  createOrUpdate('oldReviewPeriodChart', {
    type: 'bar',
    data: {
      labels: ['活动前 (6/27-7/3)', '盲盒活动中 (7/4-7/10)', '活动后 (7/11-7/18)'],
      datasets: [
        { label: '青云·新增评价', data: [5,44,34], backgroundColor: 'rgba(233,69,96,0.75)', borderRadius: 6 },
        { label: '花果园·新增评价', data: [2,77,27], backgroundColor: 'rgba(15,52,96,0.75)', borderRadius: 6 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: true,
      plugins: {
        legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } },
        tooltip: { callbacks: { label: function(ctx) { return ctx.dataset.label + ': ' + ctx.parsed.y + ' 条'; } } }
      },
      scales: {
        x: { grid: { display: false } },
        y: { beginAtZero: true, grid: { color: '#f0f0f0' }, title: { display: true, text: '评价数(条)' } }
      }
    }
  });
}

// ====== Chart 9: New store review trends ======
function initNewReview() {
  const tpDaily = [1,1,1,0,0,2,0,4,9,9];
  const fzDaily = [24,19,9,10,4,0,0,2,7,2];
  const htDaily = [0,5,12,9,2,6,9,2,3,2];
  
  let mode = 'daily';
  this.render = function() {
    if(mode === 'daily') {
      createOrUpdate('newReviewChart', {
        type: 'bar',
        data: {
          labels: newDates,
          datasets: [
            { label: '太平路', data: tpDaily, backgroundColor: 'rgba(233,69,96,0.7)', borderRadius: 3 },
            { label: '福州街', data: fzDaily, backgroundColor: 'rgba(15,52,96,0.7)', borderRadius: 3 },
            { label: '鸿通城', data: htDaily, backgroundColor: 'rgba(245,197,24,0.7)', borderRadius: 3 }
          ]
        },
        options: {
          responsive: true, maintainAspectRatio: true,
          interaction: { mode: 'index', intersect: false },
          plugins: {
            legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } }
          },
          scales: {
            x: { grid: { display: false }, stacked: true },
            y: { beginAtZero: true, grid: { color: '#f0f0f0' }, stacked: true }
          }
        }
      });
    } else {
      // Cumulative
      const tpCum = tpDaily.reduce((a,v,i) => { a.push((a[i-1]||0)+v); return a; }, []);
      const fzCum = fzDaily.reduce((a,v,i) => { a.push((a[i-1]||0)+v); return a; }, []);
      const htCum = htDaily.reduce((a,v,i) => { a.push((a[i-1]||0)+v); return a; }, []);
      createOrUpdate('newReviewChart', {
        type: 'line',
        data: {
          labels: newDates,
          datasets: [
            { label: '太平路·累计', data: tpCum, borderColor: '#e94560', backgroundColor: '#e9456020', fill: true, tension: 0.3, pointRadius: 4 },
            { label: '福州街·累计', data: fzCum, borderColor: '#0f3460', backgroundColor: '#0f346020', fill: true, tension: 0.3, pointRadius: 4 },
            { label: '鸿通城·累计', data: htCum, borderColor: '#f5c518', backgroundColor: '#f5c51820', fill: true, tension: 0.3, pointRadius: 4 }
          ]
        },
        options: {
          responsive: true, maintainAspectRatio: true,
          interaction: { mode: 'index', intersect: false },
          plugins: {
            legend: { position: 'top', labels: { usePointStyle: true, padding: 16, font: {size:12} } }
          },
          scales: {
            x: { grid: { display: false } },
            y: { beginAtZero: true, grid: { color: '#f0f0f0' } }
          }
        }
      });
    }
  };
  this.render();
  this.toggle = function(v) { mode = v; this.render(); };
  return this;
}
const _newReviewCtrl = initNewReview();

// ====== TOGGLES ======
document.getElementById('oldTrendToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    initOldTrend(e.target.dataset.view);
  }
});
document.getElementById('oldAmtToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    initOldAmt(e.target.dataset.view);
  }
});
document.getElementById('oldPeriodToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    initOldPeriod(e.target.dataset.view);
  }
});
document.getElementById('newTrendToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    initNewTrend(e.target.dataset.view);
  }
});


document.getElementById('oldReviewToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    if(_oldReviewCtrl) _oldReviewCtrl.toggle(e.target.dataset.view);
  }
});
document.getElementById('newReviewToggle')?.addEventListener('click', function(e) {
  if(e.target.tagName === 'BUTTON') {
    this.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    if(_newReviewCtrl) _newReviewCtrl.toggle(e.target.dataset.view);
  }
});

// ====== ANIMATED COUNTERS ======
function animateCounters() {
  document.querySelectorAll('.num[data-target]').forEach(el => {
    const target = parseInt(el.dataset.target);
    const duration = 2000;
    const start = performance.now();
    function update(now) {
      const pct = Math.min((now - start) / duration, 1);
      el.textContent = Math.floor(target * pct).toLocaleString();
      if(pct < 1) requestAnimationFrame(update);
      else el.textContent = target.toLocaleString();
    }
    requestAnimationFrame(update);
  });
}

// ====== NAV HIGHLIGHT ======
function updateNav() {
  const sections = document.querySelectorAll('.section, .hero');
  const links = document.querySelectorAll('nav a');
  let current = 'overview';
  sections.forEach(s => {
    const rect = s.getBoundingClientRect();
    if(rect.top <= 200) current = s.id || 'overview';
  });
  links.forEach(l => {
    l.classList.toggle('active', l.getAttribute('href') === '#' + current);
  });
}

// ====== INIT ======
document.addEventListener('DOMContentLoaded', function() {
  initOldTrend('pv');
  initOldAmt('intent');
  initOldPeriod('pv');
  initOldReview();
  initOldReviewPeriod();
  initNewReview();

  initNewTrend('pv');
  initNewCoupon();
  initNewRadar();
  animateCounters();
  
  window.addEventListener('scroll', updateNav);
  document.addEventListener('scroll', updateNav);
  
  // Intersection Observer for fade-up
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if(e.isIntersecting) e.target.style.animationPlayState = 'running';
    });
  }, { threshold: 0.1 });
  document.querySelectorAll('.fade-up').forEach(el => {
    el.style.animationPlayState = 'paused';
    observer.observe(el);
  });
});
</script>

<!-- Modal -->
<div id="chartModal" class="modal" onclick="if(event.target===this)closeModal()">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal()">✕</button>
    <h3 id="modalTitle">图表放大</h3>
    <div style="width:100%;max-height:70vh;display:flex;justify-content:center">
      <canvas id="modalCanvas" style="max-width:100%;max-height:65vh"></canvas>
    </div>
  </div>
</div>

</body>
</html>
