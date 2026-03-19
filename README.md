# Flux Image Generation API

Flux AI image generation service.

![Platform](https://img.shields.io/badge/platform-Ace%20Data%20Cloud-0f766e?style=flat-square) ![API](https://img.shields.io/badge/type-AI%20API-2563eb?style=flat-square) ![Docs](https://img.shields.io/badge/docs-online-16a34a?style=flat-square)

![Flux Image Generation](https://cdn.acedata.cloud/cdfv2n.jpg)

API home page: [Ace Data Cloud - Flux Image Generation](https://platform.acedata.cloud/service/flux)

Keywords: flux-api, ai-image, image-generation, black-forest-labs, rest-api, ai-api, aiimage, AI API, REST API, Developer API, Ace Data Cloud

## Why Use Flux Image Generation on Ace Data Cloud

- Unified developer platform with one API key, billing system, and usage tracking
- Production-ready AI API endpoints served from [https://api.acedata.cloud](https://api.acedata.cloud)
- English integration guides, API references, and service documentation
- Global-ready workflow for developers building chat, image, video, music, and search products

## Overview

<style>
.flux-page * { box-sizing: border-box; }
.flux-page h1, .flux-page h2, .flux-page h3, .flux-page h4, .flux-page h5, .flux-page h6, .flux-page p, .flux-page ul, .flux-page ol, .flux-page li, .flux-page pre, .flux-page blockquote, .flux-page table, .flux-page td, .flux-page th { margin: 0; padding: 0; }
.flux-page {
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
color: var(--el-text-color-primary);
background: var(--el-bg-color);
line-height: 1.6;
}
.flux-page a { text-decoration: none; color: inherit; }
.flux-page a:hover { text-decoration: none; }
.flux-page ul { list-style: none; }
.markdown-body .flux-page a { color: inherit !important; text-decoration: none !important; }
.markdown-body .flux-page a:hover { text-decoration: none !important; }
.markdown-body .flux-page a.fx-btn-primary,
.markdown-body .flux-page a.price-btn-fill,
.markdown-body .flux-page a.btn-cta-light { color: #ffffff !important; }
.markdown-body .flux-page a.fx-btn-secondary { color: var(--el-text-color-primary) !important; }
.markdown-body .flux-page a.price-btn-out { color: var(--el-text-color-primary) !important; }
.markdown-body .flux-page a.btn-cta-ghost { color: #94a3b8 !important; }
.markdown-body .flux-page a.btn-cta-ghost:hover { color: #e2e8f0 !important; }
.markdown-body .flux-page h1, .markdown-body .flux-page h2 { border-bottom: none !important; padding-bottom: 0 !important; }
.fx-container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }
.fx-container-narrow { max-width: 800px; margin: 0 auto; padding: 0 24px; }
.fx-section { padding: 80px 0; }
.fx-section-sm { padding: 48px 0; }
.fx-bg-white { background: var(--el-bg-color); }
.fx-bg-gray { background: var(--el-bg-color-page); }
.fx-header { text-align: center; margin-bottom: 64px; }
.fx-header h2 {
font-size: clamp(28px, 4vw, 40px);
font-weight: 700;
color: var(--el-text-color-primary);
letter-spacing: normal;
margin-bottom: 20px;
line-height: 1.15;
}
.fx-header p {
font-size: clamp(16px, 2vw, 18px);
color: var(--el-text-color-regular);
max-width: 640px;
margin: 0 auto;
line-height: 1.6;
}
.flux-page .fx-btn-primary {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: #7c3aed; color: #ffffff !important;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: background 0.2s, transform 0.15s;
border: none; cursor: pointer;
text-decoration: none !important;
}
.flux-page .fx-btn-primary:hover { background: #6d28d9; transform: translateY(-1px); text-decoration: none !important; }
.flux-page .fx-btn-secondary {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: var(--el-bg-color); color: var(--el-text-color-primary) !important;
border: 1px solid var(--el-border-color-light);
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: border-color 0.2s, background 0.2s;
cursor: pointer;
text-decoration: none !important;
}
.flux-page .fx-btn-secondary:hover { background: var(--el-bg-color-page); text-decoration: none !important; }
.flux-hero {
padding: 100px 0 80px;
text-align: center;
background: var(--el-bg-color);
position: relative;
overflow: hidden;
}
.flux-hero::before {
content: '';
position: absolute;
top: -200px; left: 50%;
transform: translateX(-50%);
width: 900px; height: 500px;
background: radial-gradient(ellipse, rgba(124, 58, 237, 0.06) 0%, transparent 70%);
pointer-events: none;
}
.fx-badge {
display: inline-flex; align-items: center; gap: 8px;
padding: 6px 16px;
background: var(--el-bg-color-page); border: 1px solid var(--el-border-color-light);
border-radius: 9999px; font-size: 13px; font-weight: 600; color: var(--el-text-color-regular);
margin-bottom: 28px;
}
.fx-badge-dot {
width: 6px; height: 6px; background: #10b981; border-radius: 50%;
display: inline-block;
}
.flux-hero h1 {
font-size: clamp(36px, 5vw, 60px);
line-height: 1.08;
letter-spacing: normal;
margin-bottom: 18px;
font-weight: 700;
}
.flux-hero h1 .fx-brand { color: #7c3aed; }
.flux-hero h1 .fx-sub { color: var(--el-text-color-primary); }
.flux-page .hero-subtitle {
max-width: 680px;
margin: 0 auto;
font-size: clamp(16px, 2.1vw, 20px);
color: var(--el-text-color-regular);
line-height: 1.6;
}
.fx-actions {
margin-top: 56px;
display: flex; gap: 12px;
justify-content: center;
flex-wrap: wrap;
}
.fx-highlights {
display: flex; gap: 24px; justify-content: center; flex-wrap: wrap;
margin-top: 40px;
}
.fx-highlights .h-item { font-size: 13px; color: var(--el-text-color-regular); display: flex; align-items: center; gap: 6px; }
.fx-highlights .h-div { width: 1px; height: 14px; background: var(--el-border-color-light); }
.fx-stats {
display: grid !important;
grid-template-columns: repeat(4, 1fr) !important;
gap: 20px !important;
}
.fx-stat {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 24px 16px;
text-align: center;
}
.fx-stat-val {
font-size: clamp(28px, 4vw, 38px);
font-weight: 700;
color: var(--el-text-color-primary);
line-height: 1.1;
}
.fx-stat-lbl { margin-top: 8px; font-size: 13px; color: var(--el-text-color-regular); }
.fx-feat-grid {
display: grid !important;
grid-template-columns: repeat(2, 1fr) !important;
gap: 20px !important;
}
.fx-feat-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 28px 24px;
transition: border-color 0.2s;
}
.fx-feat-card:hover { border-color: var(--el-text-color-regular); }
.fx-feat-card h3 { font-size: 18px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 8px; }
.fx-feat-card p { font-size: 15px; color: var(--el-text-color-regular); line-height: 1.6; }
.fx-feat-icon { font-size: 28px; margin-bottom: 12px; }
.fx-code-wrap {
border-radius: 16px !important;
overflow: hidden !important;
border: 1px solid #334155 !important;
background: #0f172a !important;
}
.markdown-body .flux-page .fx-code-wrap { background: #0f172a !important; }
.fx-code-head {
padding: 10px 16px;
background: #1e293b;
border-bottom: 1px solid #334155;
color: #cbd5e1;
font-size: 12px;
letter-spacing: 0.06em;
text-transform: uppercase;
}
.fx-code
{
margin: 0 !important;
padding: 18px !important;
white-space: pre !important;
overflow-x: auto !important;
font-family: 'JetBrains Mono', 'Fira Code', 'SFMono-Regular', monospace !important;
font-size: 13px !important;
line-height: 1.7 !important;
color: #e2e8f0 !important;
background: transparent !important;
border: none !important;
}
.markdown-body .flux-page .fx-code { background: transparent !important; }
.fx-code-grid {
display: grid !important;
grid-template-columns: 1fr 1fr !important;
gap: 0 !important;
align-items: stretch;
}
.fx-code-left {
display: flex; flex-direction: column; justify-content: center;
padding: 0 40px 0 0;
}
.fx-code-left h2 {
font-size: clamp(24px, 3.2vw, 34px);
font-weight: 700;
color: var(--el-text-color-primary);
margin-bottom: 16px;
}
.fx-code-left > p {
font-size: 16px; color: var(--el-text-color-regular); line-height: 1.6;
}
.fx-steps {
display: grid !important;
grid-template-columns: repeat(3, 1fr) !important;
gap: 0 !important;
position: relative;
}
.fx-step { text-align: center; padding: 0 24px; }
.fx-step-num {
width: 44px; height: 44px;
border-radius: 50%;
background: var(--el-bg-color-page);
border: 2px solid var(--el-border-color-light);
display: inline-flex; align-items: center; justify-content: center;
font-size: 16px; font-weight: 700; color: var(--el-text-color-regular);
margin-bottom: 16px;
}
.fx-step h3 { font-size: 17px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 8px; }
.fx-step p { font-size: 14px; color: var(--el-text-color-regular); line-height: 1.6; }
.fx-step-conn {
position: absolute;
top: 22px;
height: 2px;
background: var(--el-border-color-light);
}
.fx-step-conn-1 { left: calc(100% / 6); right: calc(100% / 6 * 3); }
.fx-step-conn-2 { left: calc(100% / 6 * 3); right: calc(100% / 6); }
.fx-uc-grid {
display: grid !important;
grid-template-columns: repeat(3, 1fr) !important;
gap: 20px !important;
}
.fx-uc-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 24px;
transition: border-color 0.2s;
}
.fx-uc-card:hover { border-color: var(--el-text-color-regular); }
.fx-uc-icon { font-size: 28px; margin-bottom: 10px; }
.fx-uc-card h3 { font-size: 16px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 6px; }
.fx-uc-card p { font-size: 14px; color: var(--el-text-color-regular); line-height: 1.5; }
.price-wrap { overflow-x: auto; -webkit-overflow-scrolling: touch; }
.flux-page .price-table {
display: table !important;
width: 100%; max-width: 700px; margin: 0 auto;
border-collapse: collapse; font-size: 15px;
}
.flux-page .price-table th, .flux-page .price-table td {
padding: 14px 18px; text-align: center;
border-bottom: 1px solid var(--el-border-color-light);
}
.flux-page .price-table th {
background: var(--el-bg-color-page);
font-weight: 600;
font-size: 13px;
color: var(--el-text-color-secondary);
text-transform: uppercase;
letter-spacing: 0.04em;
}
.flux-page .price-table td {
color: var(--el-text-color-regular);
}
.flux-page .price-table td:first-child { text-align: left; font-weight: 600; color: var(--el-text-color-primary); }
@media (max-width: 640px) {
.flux-page .price-table th, .flux-page .price-table td { padding: 10px 8px; font-size: 13px; }
} .price-hot { background: rgba(124, 58, 237, 0.04) !important; }
.price-amt { font-weight: 700; color: var(--el-text-color-primary); }
.price-cta { text-align: center; margin-top: 24px; }
.fx-faq-list { max-width: 800px; margin: 0 auto; }
.fx-faq-item { border-bottom: 1px solid var(--el-border-color-lighter); }
.fx-faq-q {
display: flex; justify-content: space-between; align-items: center;
padding: 20px 0; cursor: pointer;
font-size: 16px; font-weight: 600; color: var(--el-text-color-primary);
transition: color 0.2s;
}
.fx-faq-q:hover { color: #7c3aed; }
.fx-faq-chev { font-size: 18px; color: var(--el-text-color-regular); transition: transform 0.2s; }
.fx-faq-a { display: none; padding: 0 0 20px; }
.fx-faq-a p { font-size: 15px; color: var(--el-text-color-regular); line-height: 1.7; }
.fx-rel-grid {
display: grid !important;
grid-template-columns: repeat(4, 1fr) !important;
gap: 16px !important;
}
.fx-rel-card {
display: flex; align-items: center; gap: 14px;
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 14px;
padding: 16px 18px;
transition: border-color 0.2s;
}
.fx-rel-card:hover { border-color: var(--el-text-color-regular); }
.fx-rel-icon { font-size: 28px; flex-shrink: 0; }
.fx-rel-info h3 { font-size: 15px; font-weight: 700; color: var(--el-text-color-primary); }
.fx-rel-info p { font-size: 12px; color: var(--el-text-color-regular); margin-top: 2px; }
.fx-rel-arrow { margin-left: auto; font-size: 16px; color: var(--el-border-color); transition: color 0.2s; }
.fx-rel-card:hover .fx-rel-arrow { color: var(--el-text-color-regular); }
.flux-cta {
text-align: center;
background: #0f172a;
color: #f8fafc;
padding: 80px 0;
position: relative;
overflow: hidden;
}
.flux-cta::before {
content: '';
position: absolute;
top: -100px; left: 50%;
transform: translateX(-50%);
width: 600px; height: 400px;
background: radial-gradient(ellipse, rgba(124, 58, 237, 0.15) 0%, transparent 70%);
pointer-events: none;
}
.flux-cta h2 {
font-size: clamp(30px, 4.2vw, 46px);
font-weight: 700;
letter-spacing: normal;
margin-bottom: 28px;
position: relative;
}
.flux-cta p {
font-size: 17px;
color: #cbd5e1;
max-width: 600px;
margin: 0 auto 56px;
position: relative;
}
.flux-cta .fx-actions { position: relative; margin-top: 0; }
.flux-page .btn-cta-light {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: #7c3aed; color: #ffffff !important;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: background 0.2s;
text-decoration: none !important;
}
.flux-page .btn-cta-light:hover { background: #6d28d9; }
.flux-page .btn-cta-ghost
{
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: transparent; color: #94a3b8 !important;
border: 1px solid #475569;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: border-color 0.2s, color 0.2s;
text-decoration: none !important;
}
.flux-page .btn-cta-ghost:hover { border-color: #94a3b8; color: #e2e8f0 !important; }
.flux-page code {
background: #ede9fe !important; color: #6d28d9 !important;
border: 1px solid #c4b5fd !important;
padding: 2px 6px !important; border-radius: 4px !important;
font-size: 0.88em !important; font-weight: 600 !important;
}
html.dark .flux-page { background: var(--el-bg-color); color: var(--el-text-color-primary); }
html.dark .markdown-body .flux-page a { color: inherit !important; text-decoration: none !important; }
html.dark .markdown-body .flux-page a.fx-btn-primary,
html.dark .markdown-body .flux-page a.price-btn-fill,
html.dark .markdown-body .flux-page a.btn-cta-light { color: #ffffff !important; }
html.dark .markdown-body .flux-page a.fx-btn-secondary { color: var(--el-text-color-primary) !important; }
html.dark .markdown-body .flux-page a.price-btn-out { color: var(--el-text-color-primary) !important; }
html.dark .markdown-body .flux-page a.btn-cta-ghost { color: #94a3b8 !important; }
html.dark .markdown-body .flux-page a.btn-cta-ghost:hover { color: var(--el-text-color-primary) !important; }
html.dark .fx-bg-white { background: var(--el-bg-color); }
html.dark .fx-bg-gray { background: var(--el-bg-color-page); }
html.dark .fx-header h2 { color: var(--el-text-color-primary); }
html.dark .fx-header p { color: var(--el-text-color-secondary); }
html.dark .flux-page .fx-btn-primary { background: #7c3aed; }
html.dark .flux-page .fx-btn-primary:hover { background: #6d28d9; }
html.dark .flux-page .fx-btn-secondary { background: #1e293b; color: var(--el-text-color-primary) !important; border-color: #475569; }
html.dark .flux-page .fx-btn-secondary:hover { background: var(--el-border-color); border-color: var(--el-text-color-regular); }
html.dark .flux-hero { background: var(--el-bg-color); }
html.dark .flux-hero::before { background: radial-gradient(ellipse, rgba(124, 58, 237, 0.15) 0%, transparent 70%); }
html.dark .fx-badge { background: var(--el-bg-color-page); border-color: var(--el-border-color); color: var(--el-text-color-secondary); }
html.dark .flux-hero h1 .fx-brand { color: #a78bfa; }
html.dark .flux-hero h1 .fx-sub { color: var(--el-text-color-primary); }
html.dark .flux-page .hero-subtitle { color: var(--el-text-color-secondary); }
html.dark .fx-highlights .h-item { color: var(--el-text-color-secondary); }
html.dark .fx-highlights .h-div { background: var(--el-border-color); }
html.dark .fx-stat { background: var(--el-bg-color-page); border-color: var(--el-border-color); box-shadow: none; }
html.dark .fx-stat-val { color: var(--el-text-color-primary); }
html.dark .fx-stat-lbl { color: var(--el-text-color-regular); }
html.dark .fx-feat-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .fx-feat-card:hover { border-color: var(--el-text-color-regular); }
html.dark .fx-feat-card h3 { color: var(--el-text-color-primary); }
html.dark .fx-feat-card p { color: var(--el-text-color-secondary); }
html.dark .fx-code-left h2 { color: var(--el-text-color-primary); }
html.dark .fx-code-left > p { color: var(--el-text-color-secondary); }
html.dark .fx-step-num { background: var(--el-border-color); border-color: var(--el-text-color-regular); color: var(--el-text-color-secondary); }
html.dark .fx-step h3 { color: var(--el-text-color-primary); }
html.dark .fx-step p { color: var(--el-text-color-regular); }
html.dark .fx-step-conn { background: var(--el-border-color); }
html.dark .fx-uc-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .fx-uc-card:hover { border-color: var(--el-text-color-regular); }
html.dark .fx-uc-card h3 { color: var(--el-text-color-primary); }
html.dark .fx-uc-card p { color: var(--el-text-color-secondary); }
html.dark .flux-page .price-table th { background: var(--el-bg-color-page); color: var(--el-text-color-secondary); border-color: var(--el-border-color); }
html.dark .flux-page .price-table td { border-color: var(--el-border-color); }
html.dark .flux-page .price-table td:first-child { color: var(--el-text-color-primary); }
html.dark .price-hot { background: rgba(124, 58, 237, 0.12) !important; }
html.dark .price-amt { color: var(--el-text-color-primary); }
html.dark .fx-faq-item { border-color: var(--el-border-color); }
html.dark .fx-faq-q { color: var(--el-text-color-primary); }
html.dark .fx-faq-q:hover { color: #a78bfa; }
html.dark .fx-faq-chev { color: var(--el-text-color-regular); }
html.dark .fx-faq-a p { color: var(--el-text-color-secondary); }
html.dark .fx-rel-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .fx-rel-card:hover { border-color: var(--el-text-color-regular); }
html.dark .fx-rel-info h3 { color: var(--el-text-color-primary); }
html.dark .fx-rel-info p { color: var(--el-text-color-regular); }
html.dark .fx-rel-arrow { color: var(--el-text-color-regular); }
html.dark .flux-cta { background: #020617; }
html.dark .flux-cta::before { background: radial-gradient(ellipse, rgba(124, 58, 237, 0.2) 0%, transparent 70%); }
html.dark .flux-page .btn-cta-light { color: #ffffff !important; }
html.dark .flux-page .btn-cta-ghost { color: #94a3b8 !important; }
html.dark .flux-page .btn-cta-ghost:hover { color: var(--el-text-color-primary) !important; }
html.dark .flux-page code { background: #2e1065 !important; color: #c4b5fd !important; border-color: #6d28d9 !important; }
@media (max-width: 980px) {
.fx-stats { grid-template-columns: repeat(2, 1fr) !important; }
.fx-feat-grid { grid-template-columns: 1fr !important; }
.fx-code-grid { grid-template-columns: 1fr !important; }
.fx-code-left { padding: 0 0 32px 0; }
.fx-steps { grid-template-columns: 1fr !important; gap: 24px !important; }
.fx-step-conn { display: none; }
.fx-uc-grid { grid-template-columns: 1fr !important; }
.fx-rel-grid { grid-template-columns: repeat(2, 1fr) !important; }
}
@media (max-width: 480px) {
.fx-stats { grid-template-columns: 1fr !important; }
.fx-rel-grid { grid-template-columns: 1fr !important; }
}
</style>
<div class="flux-page">
<section class="flux-hero">
<div class="fx-container">
<div class="fx-badge"><span class="fx-badge-dot"></span>Black Forest Labs · AI Image Generation</div>
<h1><span class="fx-brand">Flux</span> <span class="fx-sub">AI Image Generation API</span></h1>
<p class="hero-subtitle">Based on the Flux series models from Black Forest Labs, quickly generate and edit high-quality AI images through a unified API, supporting the entire range of Flux Dev, Pro, and Kontext.</p>
<div class="fx-actions">
<a class="fx-btn-primary" href="/services/flux">📄 View Documentation</a>
<a class="fx-btn-secondary" href="/services/flux?tab=pricing">💰 View Pricing</a>
</div>
<div class="fx-highlights">
<span class="h-item">🎨 Text-to-Image & Image-to-Image</span>
<span class="h-div"></span>
<span class="h-item">⚡ Multiple Models Available</span>
<span class="h-div"></span>
<span class="h-item">🔗 Unified API Access</span>
</div>
</div>
</section>
<section class="fx-section-sm fx-bg-gray">
<div class="fx-container">
<div class="fx-stats">
<div class="fx-stat"><div class="fx-stat-val">6+</div><div class="fx-stat-lbl">Available Flux Models</div></div>
<div class="fx-stat"><div class="fx-stat-val">2</div><div class="fx-stat-lbl">Operation Modes: Generate & Edit</div></div>
<div class="fx-stat"><div class="fx-stat-val">$0.0095</div><div class="fx-stat-lbl">Minimum per Image</div></div>
<div class="fx-stat"><div class="fx-stat-val">0.5</div><div class="fx-stat-lbl">Free Quota</div></div>
</div>
</div>
</section>
<section class="fx-section fx-bg-white">
<div class="fx-container">
<div class="fx-header">
<h2>Core Features</h2>
<p>Covers the entire process of image generation and editing, supporting various Flux models from basic to professional.</p>
</div>
<div class="fx-feat-grid">
<div class="fx-feat-card">
<div class="fx-feat-icon">🖼️</div>
<h3>High-Quality Text-to-Image</h3>
<p>Generate high-resolution images through natural language descriptions, supporting various models including Flux Dev, Pro, and Ultra to meet different precision needs.</p>
</div>
<div class="fx-feat-card">
<div class="fx-feat-icon">✏️</div>
<h3>Kontext Image Editing</h3>
<p>Use Flux Kontext Pro/Max models for context-aware image editing, accurately modifying local details of images while maintaining overall consistency.</p>
</div>
<div class="fx-feat-card">
<div class="fx-feat-icon">🔄</div>
<h3>Asynchronous Task Architecture</h3>
<p>After submitting a generation request, poll for results using Task ID; the polling interface is completely free, suitable for batch and background processing scenarios.</p>
</div>
<div class="fx-feat-card">
<div class="fx-feat-icon">📐</div>
<h3>Flexible Parameter Control</h3>
<p>Supports custom width, height, seed, quantity, and other parameters, allowing precise control over generation results to meet product-level image production needs.</p>
</div>
</div>
</div>
</section>
<section class="fx-section fx-bg-gray">
<div class="fx-container">
<div class="fx-code-grid">
<div class="fx-code-left">
<h2>Generate Images with a Single Request</h2>
<p>Use the standard RESTful API, authenticate via Bearer Token, and quickly submit image generation tasks.</p>
</div>
<div>
<div class="fx-code-wrap">
<div class="fx-code-head">cURL</div>
<pre class="fx-code">curl -X POST "https://api.acedata.cloud/flux/images" \
-H "Authorization: Bearer YOUR_API_KEY" \
-H "Content-Type: application/json" \
-d '{
"model": "flux-pro-1.1",
"prompt": "A futuristic cityscape at sunset",
"width": 1024,
"height": 1024,
"count": 1
}'</pre>
</div>
</div>
</div>
</div>
</section>
<section class="fx-section fx-bg-white">
<div class="fx-container">
<div class="fx-header">
<h2>3 Steps to Get Started Quickly</h2>
<p>From registration to generating the first image, it only takes a few minutes.</p>
</div>
<div class="fx-steps">
<div class="fx-step-conn fx-step-conn-1"></div>
<div class="fx-step-conn fx-step-conn-2"></div>
<div class="fx-step">
<div class="fx-step-num">1</div>
<h3>Register and Obtain API Key</h3>
<p>Create an account on the platform and go to the console to generate an API key.</p>
</div>
<div class="fx-step">
<div class="fx-step-num">2</div>
<h3>Call the Image Generation API</h3>
<p>Use the <code>/flux/images</code> endpoint to submit a generation request and obtain a Task ID.</p>
</div>
<div class="fx-step">
<div class="fx-step-num">3</div>
<h3>Poll for Results</h3>
<p>Check the task status via <code>/flux/tasks</code> to obtain the generated image.</p>
</div>
</div>
</div>
</section>
<section class="fx-section fx-bg-gray">
<div class="fx-container">
<div class="fx-header">
<h2>What is Flux Suitable For?</h2>
<p>Various application scenarios to meet creative design and product-level image needs.</p>
</div>
<div class="fx-uc-grid">
<div class="fx-uc-card">
<div class="fx-uc-icon">🎨</div>
<h3>Creative Design</h3>
<p>Quickly generate concept art, illustrations, and design materials to accelerate creative workflows.</p>
</div>
<div class="fx-uc-card">
<div class="fx-uc-icon">🛒</div>
<h3>E-commerce Product Images</h3>
<p>Batch generate product display images and scene images to reduce shooting costs.</p>
</div>
<div class="fx-uc-card">
<div class="fx-uc-icon">📱</div>
<h3>Social Media Content</h3>
<p>Quickly produce high-quality graphic content to enhance social media operation efficiency.</p>
</div>
<div class="fx-uc-card">
<div class="fx-uc-icon">🎮</div>
<h3>Games and Entertainment</h3>
<p>Generate game characters and scene concept art to assist in game development and content creation.</p>
</div>
<div class="fx-uc-card">
<div class="fx-uc-icon">📝</div>
<h3>Image Editing</h3>
<p>Use Kontext models for local editing, modifying specific elements within images.</p>
</div>
<div class="fx-uc-card">
<div class="fx-uc-icon">🏢</div>
<h3>Corporate Marketing</h3>
<p>Quickly produce advertising concepts, marketing materials, and brand visual content.</p>
</div>
</div>
</div>
</section>
<section class="fx-section fx-bg-white">
<div class="fx-container">
<div class="fx-header">
<h2>Flux API Pricing</h2>
<p>Charged based on the number of images generated, with prices depending on the model. The polling interface is completely free.</p>
</div>
<div class="price-wrap">
<table class="price-table">
<thead>
<tr>
<th>Model</th>
<th>Price per Image</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Flux Dev</td>
<td><span class="price-amt">$0.0095</span></td>
<td>For development and testing</td>
</tr>
<tr class="price-hot">
<td>Flux Pro 1.1</td>
<td><span class="price-amt">$0.015</span></td>
<td>Recommended · High-Quality Production</td>
</tr>
<tr>
<td>Flux Pro</td>
<td><span class="price-amt">$0.019</span></td>
<td>Professional-Level Generation</td>
</tr>
<tr>
<td>Flux Pro 1.1 Ultra</td>
<td><span class="price-amt">$0.023</span></td>
<td>Ultra High Resolution</td>
</tr>
<tr class="price-hot">
<td>Flux Kontext Pro</td>
<td><span class="price-amt">$0.015</span></td>
<td>Recommended · Context Editing</td>
</tr>
<tr>
<td>Flux Kontext Max</td>
<td><span class="price-amt">$0.030</span></td>
<td>Strongest Editing Capability</td>
</tr>
</tbody>
</table>
</div>
<div class="price-cta">
<p style="font-size:14px;color:#94a3b8;margin-top:16px;">For multiple images generated, charged by the <code>count</code> quantity multiplier · Polling task status interface <code>/flux/tasks</code> is completely free</p>
</div>
</div>
</section>
<section class="fx-section fx-bg-gray">
<div class="fx-container-narrow">
<div class="fx-header">
<h2>Frequently Asked Questions</h2>
<p>Common questions about using the Flux API</p>
</div>
<div class="fx-faq-list">
<div class="fx-faq-item">
<div class="fx-faq-q"><span>What is the difference between Flux Dev and Flux Pro?</span><span class="fx-faq-chev">›</span></div>
<div class="fx-faq-a"><p>Flux Dev is a model for development and testing, with the lowest price ($0.0095/image). Flux Pro and Pro 1.1 are production-level models, offering higher image quality and richer details, recommended for formal products.</p></div>
</div>
<div class="fx-faq-item">
<div class="fx-faq-q"><span>What is the Kontext model for?</span><span class="fx-faq-chev">›</span></div>
<div class="fx-faq-a"><p>The Flux Kontext model supports context-aware image editing—input an existing image along with text instructions to accurately modify specific parts of the image without affecting other areas.</p></div>
</div>
<div class="fx-faq-item">
<div class="fx-faq-q"><span>Is polling the task status charged?</span><span class="fx-faq-chev">›</span></div>
<div class="fx-faq-a"><p>No charge. The <code>/flux/tasks</code> interface is completely free; charges only apply when submitting a generation request.</p></div>
</div>
<div class="fx-faq-item">
<div class="fx-faq-q"><span>What image sizes are supported?</span><span class="fx-faq-chev">›</span></div>
<div class="fx-faq-a"><p>Custom width and height parameters are supported. Common sizes include 1024×1024, 1024×1792, 1792×1024, etc. The Ultra model supports higher resolutions.</p></div>
</div>
<div class="fx-faq-item">
<div class="fx-faq-q"><span>Can images be generated in bulk?</span><span class="fx-faq-chev">›</span></div>
<div class="fx-faq-a"><p>Yes. Specify the number of images to generate per request using the <code>count</code> parameter, with costs calculated by quantity multiplier.</p></div>
</div>
</div>
</div>
</section>
<section class="fx-section fx-bg-white">
<div class="fx-container">
<div class="fx-header">
<h2>Explore More AI Image Services</h2>
<p>Ace Data Cloud offers various AI image generation and editing services</p>
</div>
<div class="fx-rel-grid">
<a class="fx-rel-card" href="/services/midjourney">
<span class="fx-rel-icon">🎨</span>
<div class="fx-rel-info"><h3>Midjourney</h3><p>Top AI Painting</p></div>
<span class="fx-rel-arrow">→</span>
</a>
<a class="fx-rel-card" href="/services/nano-banana">
<span class="fx-rel-icon">🍌</span>
<div class="fx-rel-info"><h3>Nano Banana</h3><p>Gemini Image Generation</p></div>
<span class="fx-rel-arrow">→</span>
</a>
<a class="fx-rel-card" href="/services/seedream">
<span class="fx-rel-icon">🌱</span>
<div class="fx-rel-info"><h3>Seedream</h3><p>ByteDance Image Generation</p></div>
<span class="fx-rel-arrow">→</span>
</a>
<a class="fx-rel-card" href="/services/sora">
<span class="fx-rel-icon">🎬</span>
<div class="fx-rel-info"><h3>Sora</h3><p>OpenAI Video Generation</p></div>
<span class="fx-rel-arrow">→</span>
</a>
</div>
</div>
</section>
<section class="flux-cta">
<div class="fx-container">
<h2>Start Using Flux API Now</h2>
<p>Unified access to the entire range of Flux models, easily integrate AI image generation capabilities into your products.</p>
<div class="fx-actions">
<a class="btn-cta-light" href="/services/flux">Get Started →</a>
<a class="btn-cta-ghost" href="/support">Contact Support</a>
</div>
</div>
</section>
</div>

## Quick Start

- Base URL: [https://api.acedata.cloud](https://api.acedata.cloud)
- Service page: [Flux Image Generation on Ace Data Cloud](https://platform.acedata.cloud/service/flux)
- Docs: [Developer documentation](https://docs.acedata.cloud)

```bash
curl --request POST "https://api.acedata.cloud/flux/images" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{}'
```

## APIs and Guides

Explore the supported endpoints and integration guides for Flux Image Generation.

| API | Path | Integration Guidance |
| ---- | ---- | ------------ |
| [Flux Images Generation API](https://platform.acedata.cloud/documents/6b9197c5-7a3f-4878-a43f-7f94e7e66394) | `/flux/images` | [Flux Images Generation API Integration Guide](https://platform.acedata.cloud/documents/92754994-3970-4f2a-9bf3-113149c25c11) |
| [Flux Tasks API](https://platform.acedata.cloud/documents/39b38bbe-60f3-40da-b2b6-5ce1e091852b) | `/flux/tasks` | [Flux Tasks API Integration Guide](https://platform.acedata.cloud/documents/5998d153-b9a6-4798-b5d9-8f523d0d626f) |

## Related Resources

- [Ace Data Cloud Developer Platform](https://platform.acedata.cloud)
- [Ace Data Cloud Docs](https://docs.acedata.cloud)
- [Status Page](https://status.acedata.cloud)
- [Ace Data Cloud GitHub Organization](https://github.com/AceDataCloud)

## Support

If you meet any issue, please check [support info](https://platform.acedata.cloud/support) or browse the latest documentation on [docs.acedata.cloud](https://docs.acedata.cloud).