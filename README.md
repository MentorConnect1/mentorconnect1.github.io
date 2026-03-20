<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mentor Connect</title>
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2.45.4/dist/umd/supabase.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400&display=swap" rel="stylesheet" />
  <style>
    :root {
      --blue-50: #eff6ff;
      --blue-100: #dbeafe;
      --blue-200: #bfdbfe;
      --blue-300: #93c5fd;
      --blue-400: #60a5fa;
      --blue-500: #3b82f6;
      --blue-600: #2563eb;
      --blue-700: #1d4ed8;
      --blue-800: #1e40af;
      --blue-900: #1e3a8a;
      --red-50: #fef2f2;
      --red-200: #fecaca;
      --red-600: #dc2626;
      --red-700: #b91c1c;
      --green-50: #f0fdf4;
      --green-200: #bbf7d0;
      --green-600: #16a34a;
      --green-700: #15803d;
      --amber-50: #fffbeb;
      --amber-200: #fde68a;
      --amber-600: #d97706;
      --white: #ffffff;
      --shadow-sm: 0 1px 3px rgba(0,0,0,.07);
      --shadow: 0 4px 18px rgba(37,99,235,.10);
      --shadow-lg: 0 8px 32px rgba(37,99,235,.16);
      --shadow-xl: 0 16px 48px rgba(37,99,235,.20);
      --radius: 12px;
      --radius-xl: 18px;
      --transition: all .2s cubic-bezier(.4,0,.2,1);
      --sidebar-w: 220px;
    }
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'DM Sans', sans-serif;
      background: linear-gradient(135deg, var(--blue-50) 0%, #fff 50%, #e0eaff 100%);
      min-height: 100vh;
      color: var(--blue-900);
      -webkit-font-smoothing: antialiased;
      overflow-x: hidden;
    }
    /* Phone view toggle */
    body.phone-view {
      max-width: 375px;
      margin: 0 auto;
      box-shadow: 0 0 0 1px #e5e7eb;
    }

    .page { display: none; min-height: 100vh; animation: fadeUp .28s ease both; }
    .page.active { display: block; }

    /* ── AUTH ── */
    .auth-wrap { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 2rem 1rem; }
    .auth-inner { width: 100%; max-width: 440px; }
    .logo-block { text-align: center; margin-bottom: 1.75rem; }
    .logo-icon { width: 62px; height: 62px; background: linear-gradient(135deg, var(--blue-600), var(--blue-800)); border-radius: 18px; display: flex; align-items: center; justify-content: center; margin: 0 auto .9rem; box-shadow: 0 8px 28px rgba(37,99,235,.38); }
    .logo-icon svg { width: 34px; height: 34px; stroke: white; fill: none; stroke-width: 2; }
    .logo-title { font-family: 'DM Serif Display', serif; font-size: 2.1rem; color: var(--blue-900); letter-spacing: -.02em; }
    .logo-sub { color: rgba(37,99,235,.55); margin-top: .2rem; font-size: .9rem; }
    .card { background: rgba(255,255,255,.93); backdrop-filter: blur(14px); border-radius: var(--radius-xl); box-shadow: var(--shadow-xl); border: 1px solid rgba(255,255,255,.8); }
    .card-header { padding: 1.6rem 1.8rem 0; }
    .card-body { padding: 1.4rem 1.8rem 1.8rem; }
    .card-title { font-family: 'DM Serif Display', serif; font-size: 1.55rem; color: var(--blue-900); }
    .card-sub { color: rgba(37,99,235,.6); font-size: .85rem; margin-top: .2rem; }

    /* ── BUTTONS ── */
    .btn { display: inline-flex; align-items: center; justify-content: center; gap: .45rem; padding: .72rem 1.4rem; font-family: 'DM Sans', sans-serif; font-size: .92rem; font-weight: 600; border-radius: var(--radius); border: none; cursor: pointer; transition: var(--transition); text-decoration: none; width: 100%; letter-spacing: .01em; }
    .btn svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 2; flex-shrink: 0; }
    .btn-primary { background: var(--blue-600); color: #fff; }
    .btn-primary:hover:not(:disabled) { background: var(--blue-700); transform: translateY(-1px); box-shadow: 0 4px 18px rgba(37,99,235,.4); }
    .btn-primary:disabled { opacity: .55; cursor: not-allowed; }
    .btn-outline { background: transparent; border: 2px solid var(--blue-200); color: var(--blue-700); }
    .btn-outline:hover { background: var(--blue-50); border-color: var(--blue-400); }
    .btn-ghost { background: transparent; color: var(--blue-600); width: auto; padding: .45rem .75rem; }
    .btn-ghost:hover { background: var(--blue-50); }
    .btn-danger-outline { background: transparent; border: 2px solid #fca5a5; color: var(--red-600); }
    .btn-danger-outline:hover { background: var(--red-50); }
    .btn-sm { padding: .38rem .8rem; font-size: .82rem; width: auto; }
    .btn-icon { padding: .48rem; width: auto; min-width: 2.1rem; height: 2.1rem; }
    .btn-success { background: var(--green-600); color: white; }
    .btn-success:hover { background: var(--green-700); }
    .btn-back { display: inline-flex; align-items: center; gap: .3rem; background: none; border: none; color: var(--blue-400); font-size: .83rem; font-family: 'DM Sans', sans-serif; cursor: pointer; padding: 0; margin-bottom: .65rem; transition: color .15s; }
    .btn-back:hover { color: var(--blue-700); }
    .btn-back svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2.2; }

    /* ── FORMS ── */
    .form-group { margin-bottom: .9rem; position: relative; }
    .form-label { display: block; font-size: .83rem; font-weight: 500; color: var(--blue-900); margin-bottom: .3rem; }
    .form-input, .form-textarea, .form-select { width: 100%; padding: .62rem .9rem; border: 1.5px solid var(--blue-200); border-radius: var(--radius); font-family: 'DM Sans', sans-serif; font-size: .89rem; color: var(--blue-900); background: white; transition: var(--transition); outline: none; appearance: none; }
    .form-input:focus, .form-textarea:focus, .form-select:focus { border-color: var(--blue-500); box-shadow: 0 0 0 3px rgba(37,99,235,.11); }
    .form-input::placeholder { color: rgba(37,99,235,.3); }
    .form-textarea { resize: vertical; min-height: 96px; line-height: 1.5; }
    .form-select { background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' viewBox='0 0 24 24' fill='none' stroke='%232563eb' stroke-width='2'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right .75rem center; padding-right: 2.4rem; }
    .input-wrap { position: relative; }
    .input-eye { position: absolute; right: .75rem; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer; color: var(--blue-400); display: flex; padding: .2rem; }
    .input-eye svg { width: 17px; height: 17px; stroke: currentColor; fill: none; stroke-width: 2; }
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: .7rem; }
    .alert { padding: .72rem 1rem; border-radius: var(--radius); font-size: .85rem; margin-bottom: .9rem; }
    .alert-error { background: var(--red-50); border: 1px solid var(--red-200); color: var(--red-700); }
    .alert-success { background: var(--green-50); border: 1px solid var(--green-200); color: var(--green-700); display: flex; align-items: center; gap: .5rem; }
    .alert-success svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 2.5; flex-shrink: 0; }
    .alert-info { background: var(--blue-50); border: 1px solid var(--blue-200); color: var(--blue-800); }
    .divider-text { text-align: center; color: rgba(37,99,235,.5); font-size: .83rem; margin-top: .9rem; }
    .divider-text a, .divider-text button { color: var(--blue-600); font-weight: 600; text-decoration: none; background: none; border: none; cursor: pointer; font-family: inherit; font-size: inherit; }
    .divider-text a:hover, .divider-text button:hover { text-decoration: underline; }
    .role-grid { display: grid; grid-template-columns: 1fr 1fr; gap: .7rem; margin-top: 1.5rem; }
    .role-tile { background: rgba(255,255,255,.65); border-radius: var(--radius); padding: .9rem; text-align: center; border: 1.5px solid var(--blue-100); box-shadow: var(--shadow-sm); }
    .role-tile svg { width: 28px; height: 28px; stroke: var(--blue-600); fill: none; stroke-width: 1.75; margin: 0 auto .45rem; display: block; }
    .role-tile h3 { font-weight: 600; font-size: .88rem; color: var(--blue-900); }
    .role-tile p { font-size: .72rem; color: rgba(37,99,235,.5); margin-top: .15rem; }
    .role-option { width: 100%; padding: .85rem 1rem; border: 2px solid var(--blue-100); border-radius: var(--radius); background: white; cursor: pointer; display: flex; align-items: center; gap: .85rem; text-align: left; transition: var(--transition); font-family: 'DM Sans', sans-serif; margin-bottom: .55rem; }
    .role-option:last-of-type { margin-bottom: 0; }
    .role-option:hover { border-color: var(--blue-300); background: var(--blue-50); }
    .role-option.selected { border-color: var(--blue-600); background: #eff6ff; }
    .role-option-icon { width: 38px; height: 38px; border-radius: 10px; background: var(--blue-100); display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: var(--transition); }
    .role-option.selected .role-option-icon { background: var(--blue-600); }
    .role-option-icon svg { width: 18px; height: 18px; stroke: var(--blue-600); fill: none; stroke-width: 2; transition: var(--transition); }
    .role-option.selected .role-option-icon svg { stroke: white; }
    .role-option-text h3 { font-weight: 600; color: var(--blue-900); font-size: .88rem; }
    .role-option-text p { font-size: .73rem; color: rgba(37,99,235,.55); }
    .role-check { margin-left: auto; color: var(--blue-600); display: none; }
    .role-check svg { width: 17px; height: 17px; stroke: currentColor; fill: none; stroke-width: 2.5; }
    .role-option.selected .role-check { display: block; }
    .checkbox-wrap { display: flex; align-items: center; gap: .45rem; margin-bottom: .6rem; }
    .checkbox-wrap input[type="checkbox"] { width: 15px; height: 15px; accent-color: var(--blue-600); cursor: pointer; flex-shrink: 0; }
    .checkbox-wrap label { font-size: .85rem; color: var(--blue-900); cursor: pointer; user-select: none; }

    /* ── AUTOCOMPLETE ── */
    .autocomplete-wrap { position: relative; }
    .autocomplete-dropdown { position: absolute; left: 0; right: 0; top: calc(100% + 3px); background: white; border: 1.5px solid var(--blue-200); border-radius: var(--radius); box-shadow: var(--shadow-lg); z-index: 999; max-height: 220px; overflow-y: auto; display: none; }
    .autocomplete-dropdown.open { display: block; }
    .autocomplete-item { padding: .6rem 1rem; font-size: .88rem; color: var(--blue-900); cursor: pointer; transition: background .12s; border-bottom: 1px solid var(--blue-50); }
    .autocomplete-item:last-child { border-bottom: none; }
    .autocomplete-item:hover, .autocomplete-item.focused { background: var(--blue-50); color: var(--blue-700); }
    .autocomplete-item mark { background: none; color: var(--blue-600); font-weight: 700; }
    .autocomplete-no-results { padding: .6rem 1rem; font-size: .84rem; color: rgba(37,99,235,.4); font-style: italic; }

    /* ── VERIFY ── */
    .verify-digits { display: flex; gap: .6rem; justify-content: center; margin: 1.2rem 0; }
    .verify-digit { width: 52px; height: 60px; border: 2px solid var(--blue-200); border-radius: var(--radius); font-size: 1.6rem; font-weight: 700; font-family: 'DM Serif Display', serif; color: var(--blue-900); text-align: center; outline: none; transition: var(--transition); background: white; }
    .verify-digit:focus { border-color: var(--blue-500); box-shadow: 0 0 0 3px rgba(37,99,235,.12); }
    .verify-digit.filled { border-color: var(--blue-400); background: var(--blue-50); }
    .verify-hint { background: var(--amber-50); border: 1px solid var(--amber-200); border-radius: var(--radius); padding: .75rem 1rem; font-size: .83rem; color: var(--amber-600); margin-bottom: 1rem; display: flex; align-items: center; gap: .5rem; }
    .verify-hint svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 2; flex-shrink: 0; }
    .verify-code-display { font-family: 'DM Serif Display', serif; font-size: 1.6rem; color: var(--blue-700); letter-spacing: .25em; text-align: center; margin: .5rem 0; }

    /* ── APP LAYOUT ── */
    .app-layout { min-height: 100vh; display: flex; flex-direction: column; }
    .top-bar { background: rgba(255,255,255,.88); backdrop-filter: blur(16px); border-bottom: 1px solid var(--blue-100); position: sticky; top: 0; z-index: 40; }
    .top-bar-inner { padding: 1rem 1.2rem; }
    .top-bar-title { display: flex; align-items: center; gap: .7rem; }
    .top-bar-title h1 { font-family: 'DM Serif Display', serif; font-size: 1.45rem; color: var(--blue-900); }
    .top-bar-title svg { width: 22px; height: 22px; stroke: var(--blue-600); fill: none; stroke-width: 2; }
    .page-content { flex: 1; padding: 1.2rem 1.2rem 6.5rem; width: 100%; }

    /* ── SIDEBAR NAV (desktop default) ── */
    .bottom-nav {
      position: fixed;
      left: 0; top: 0; bottom: 0;
      width: var(--sidebar-w);
      background: rgba(255,255,255,.94);
      backdrop-filter: blur(18px);
      border-right: 1px solid var(--blue-100);
      z-index: 50;
      display: flex;
      flex-direction: column;
    }
    .bottom-nav-inner {
      display: flex;
      flex-direction: column;
      padding: 1.5rem .75rem;
      gap: .3rem;
      flex: 1;
    }
    .sidebar-logo {
      display: flex;
      align-items: center;
      gap: .65rem;
      padding: .5rem .6rem 1.2rem;
      border-bottom: 1px solid var(--blue-100);
      margin-bottom: .5rem;
    }
    .sidebar-logo-icon {
      width: 34px; height: 34px;
      background: linear-gradient(135deg, var(--blue-600), var(--blue-800));
      border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
    }
    .sidebar-logo-icon svg { width: 18px; height: 18px; stroke: white; fill: none; stroke-width: 2; }
    .sidebar-logo span { font-family: 'DM Serif Display', serif; font-size: 1rem; color: var(--blue-900); font-weight: 400; }
    .nav-item {
      display: flex;
      flex-direction: row;
      align-items: center;
      gap: .7rem;
      padding: .62rem .85rem;
      border-radius: 11px;
      color: var(--blue-400);
      font-size: .875rem;
      font-weight: 500;
      transition: var(--transition);
      cursor: pointer;
      border: none;
      background: none;
      font-family: 'DM Sans', sans-serif;
      position: relative;
      text-align: left;
      width: 100%;
    }
    .nav-item:hover { color: var(--blue-700); background: var(--blue-50); }
    .nav-item.active { color: var(--blue-700); background: var(--blue-50); font-weight: 600; }
    .nav-item svg { width: 18px; height: 18px; stroke: currentColor; fill: none; stroke-width: 2; flex-shrink: 0; }
    .nav-item.active svg { stroke-width: 2.5; }
    .nav-badge { background: #ef4444; color: white; font-size: .62rem; font-weight: 700; min-width: 18px; height: 18px; border-radius: 99px; display: flex; align-items: center; justify-content: center; padding: 0 4px; margin-left: auto; }
    /* With sidebar: shift content right */
    .app-layout .top-bar-inner { padding-left: calc(var(--sidebar-w) + 1.2rem); }
    .app-layout .page-content { padding-left: calc(var(--sidebar-w) + 1.2rem); padding-bottom: 2.5rem; }

    /* Phone view overrides sidebar → bottom nav */
    body.phone-view .bottom-nav {
      position: fixed;
      left: 50%; transform: translateX(-50%);
      top: auto; bottom: 0;
      width: 100%; max-width: 375px;
      height: auto;
      border-right: none;
      border-top: 1px solid var(--blue-100);
      flex-direction: row;
    }
    body.phone-view .bottom-nav-inner {
      flex-direction: row;
      justify-content: space-around;
      padding: .35rem .4rem;
      gap: 0;
    }
    body.phone-view .sidebar-logo { display: none; }
    body.phone-view .nav-item {
      flex-direction: column;
      align-items: center;
      gap: .17rem;
      padding: .45rem .55rem;
      font-size: .67rem;
      min-width: 48px;
      width: auto;
    }
    body.phone-view .nav-badge { position: absolute; top: 1px; right: 2px; margin-left: 0; }
    body.phone-view .app-layout .top-bar-inner { padding-left: 1.2rem; }
    body.phone-view .app-layout .page-content { padding-left: 1.2rem; padding-bottom: 6.5rem; }
    body.phone-view .page-content { max-width: 100%; }

    /* Wide content on desktop */
    .page-content-inner { max-width: 1100px; }

    /* Person grid - 2 columns on desktop, 1 on phone */
    .person-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(340px, 1fr)); gap: 1rem; }
    body.phone-view .person-grid { grid-template-columns: 1fr; }

    /* ── SEARCH & FILTERS ── */
    .search-bar-wrap { position: relative; margin-bottom: .9rem; }
    .search-icon { position: absolute; left: .75rem; top: 50%; transform: translateY(-50%); pointer-events: none; }
    .search-icon svg { width: 17px; height: 17px; stroke: var(--blue-400); fill: none; stroke-width: 2; }
    .search-bar-wrap .form-input { padding-left: 2.4rem; }
    .filter-row { display: flex; gap: .5rem; margin-bottom: .9rem; }
    .filter-row .form-select { flex: 1; }

    /* ── PERSON CARDS (redesigned) ── */
    .person-card {
      background: rgba(255,255,255,.92);
      backdrop-filter: blur(8px);
      border-radius: var(--radius-xl);
      box-shadow: var(--shadow);
      padding: 1.25rem 1.3rem;
      transition: var(--transition);
      border: 1px solid rgba(255,255,255,.8);
      position: relative;
      overflow: hidden;
    }
    .person-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
      background: linear-gradient(90deg, var(--blue-400), var(--blue-600));
      opacity: 0;
      transition: opacity .2s;
    }
    .person-card:hover { box-shadow: var(--shadow-lg); transform: translateY(-2px); }
    .person-card:hover::before { opacity: 1; }
    .person-card-top { display: flex; align-items: flex-start; gap: 1rem; margin-bottom: .85rem; }
    .avatar {
      width: 52px; height: 52px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--blue-500), var(--blue-700));
      display: flex; align-items: center; justify-content: center;
      color: white; font-size: 1rem; font-weight: 600;
      flex-shrink: 0;
      font-family: 'DM Serif Display', serif;
      box-shadow: 0 4px 12px rgba(37,99,235,.25);
    }
    .avatar-sm { width: 42px; height: 42px; font-size: .9rem; }
    .avatar-lg { width: 60px; height: 60px; font-size: 1.3rem; }
    .person-card-info { flex: 1; min-width: 0; }
    .person-name { font-weight: 700; font-size: 1.02rem; color: var(--blue-900); line-height: 1.2; }
    .person-role-pill {
      display: inline-flex; align-items: center; gap: .25rem;
      background: var(--blue-50);
      color: var(--blue-700);
      font-size: .72rem; font-weight: 600;
      padding: .18rem .6rem;
      border-radius: 99px;
      border: 1px solid var(--blue-100);
      margin-top: .3rem;
      text-transform: capitalize;
    }
    .person-meta { display: flex; flex-wrap: wrap; gap: .5rem; margin-top: .55rem; }
    .meta-item { display: flex; align-items: center; gap: .25rem; font-size: .78rem; color: rgba(37,99,235,.65); background: var(--blue-50); padding: .18rem .55rem; border-radius: 99px; }
    .meta-item svg { width: 12px; height: 12px; stroke: currentColor; fill: none; stroke-width: 2.2; }
    .person-desc { font-size: .84rem; color: rgba(30,58,138,.72); line-height: 1.6; display: -webkit-box; -webkit-line-clamp: 2; line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
    .person-card-footer { display: flex; align-items: center; justify-content: space-between; margin-top: .9rem; padding-top: .75rem; border-top: 1px solid var(--blue-50); }
    .person-card-actions { display: flex; gap: .45rem; align-items: center; }
    .avail-badge { display: inline-flex; align-items: center; gap: .25rem; padding: .2rem .65rem; border-radius: 99px; font-size: .72rem; font-weight: 600; }
    .avail-yes { background: var(--green-50); color: var(--green-700); border: 1px solid var(--green-200); }
    .avail-no { background: #f3f4f6; color: #6b7280; border: 1px solid #e5e7eb; }
    .tabroom-badge { display: inline-flex; align-items: center; gap: .22rem; background: #f0fdf4; color: #166534; border: 1px solid #bbf7d0; padding: .2rem .6rem; border-radius: 99px; font-size: .71rem; font-weight: 600; }
    .tabroom-badge svg { width: 11px; height: 11px; stroke: currentColor; fill: none; stroke-width: 2.5; }
    .msg-btn {
      display: inline-flex; align-items: center; gap: .35rem;
      background: var(--blue-600); color: white;
      border: none; border-radius: 10px;
      padding: .42rem .9rem; font-size: .8rem; font-weight: 600;
      cursor: pointer; transition: var(--transition);
      font-family: 'DM Sans', sans-serif;
    }
    .msg-btn svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2; }
    .msg-btn:hover { background: var(--blue-700); transform: translateY(-1px); }
    .msg-btn.msg-hidden {
      background: var(--blue-50);
      color: var(--blue-600);
      border: 1.5px solid var(--blue-200);
    }
    .msg-btn.msg-hidden:hover { background: var(--blue-100); }

    /* ── CONVERSATIONS ── */
    .convo-card { background: rgba(255,255,255,.88); backdrop-filter: blur(8px); border-radius: var(--radius-xl); box-shadow: var(--shadow); padding: .95rem 1.1rem; margin-bottom: .7rem; cursor: pointer; transition: var(--transition); border: 1px solid rgba(255,255,255,.7); }
    .convo-card:hover { box-shadow: var(--shadow-lg); transform: translateY(-1px); }
    .convo-card.unread { background: rgba(219,234,254,.55); }
    .convo-inner { display: flex; align-items: center; gap: .8rem; }
    .convo-meta { flex: 1; min-width: 0; }
    .convo-row1 { display: flex; justify-content: space-between; align-items: center; }
    .convo-name { font-weight: 600; color: var(--blue-900); font-size: .93rem; }
    .convo-card.unread .convo-name { font-weight: 700; }
    .convo-date { font-size: .72rem; color: rgba(37,99,235,.5); }
    .convo-row2 { display: flex; align-items: center; justify-content: space-between; margin-top: .18rem; }
    .convo-preview { font-size: .83rem; color: rgba(37,99,235,.55); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 200px; }
    .convo-card.unread .convo-preview { color: var(--blue-800); font-weight: 500; }
    .convo-end { display: flex; align-items: center; gap: .35rem; }
    .unread-dot { width: 9px; height: 9px; background: var(--blue-600); border-radius: 50%; }
    .chev-icon svg { width: 14px; height: 14px; stroke: var(--blue-400); fill: none; stroke-width: 2; }
    .hide-btn {
      background: none; border: none; cursor: pointer;
      color: #aaa; padding: .3rem; border-radius: 8px;
      transition: var(--transition); display: flex; align-items: center;
      margin-left: .2rem;
    }
    .hide-btn:hover { color: var(--red-600); background: var(--red-50); }
    .hide-btn svg { width: 15px; height: 15px; stroke: currentColor; fill: none; stroke-width: 2; }

    /* ── CHAT ── */
    .chat-wrap { display: flex; flex-direction: column; height: 100vh; }
    .chat-header { background: rgba(255,255,255,.92); backdrop-filter: blur(14px); border-bottom: 1px solid var(--blue-100); padding: .9rem 1.2rem; display: flex; align-items: center; gap: .75rem; }
    .chat-header-info h2 { font-weight: 600; font-size: .98rem; color: var(--blue-900); }
    .chat-header-info p { font-size: .76rem; color: rgba(37,99,235,.5); text-transform: capitalize; }
    .chat-messages { flex: 1; overflow-y: auto; padding: 1.2rem; display: flex; flex-direction: column; gap: .55rem; }
    .message { display: flex; }
    .message.mine { justify-content: flex-end; }
    /* FIXED: bubble sizing uses inline-flex so it wraps content exactly */
    .message > div {
      max-width: 74%;
      display: inline-flex;
      flex-direction: column;
      align-items: flex-start;
    }
    .message.mine > div { align-items: flex-end; }
    .message-bubble {
      display: inline-block;
      padding: .62rem .88rem;
      border-radius: 16px;
      font-size: .88rem;
      line-height: 1.45;
      word-break: break-word;
      max-width: 100%;
    }
    .message.other .message-bubble { background: white; color: var(--blue-900); box-shadow: var(--shadow-sm); border-bottom-left-radius: 4px; }
    .message.mine .message-bubble { background: var(--blue-600); color: white; border-bottom-right-radius: 4px; }
    .message-time { font-size: .68rem; color: rgba(37,99,235,.4); margin-top: .22rem; }
    .message.mine .message-time { color: rgba(255,255,255,.6); text-align: right; }
    .chat-input-bar { background: rgba(255,255,255,.92); backdrop-filter: blur(14px); border-top: 1px solid var(--blue-100); padding: .7rem 1.1rem; display: flex; gap: .55rem; align-items: flex-end; }
    .chat-input-bar textarea { flex: 1; border: 1.5px solid var(--blue-200); border-radius: 12px; padding: .58rem .85rem; font-family: 'DM Sans', sans-serif; font-size: .88rem; resize: none; max-height: 96px; outline: none; transition: var(--transition); background: white; color: var(--blue-900); }
    .chat-input-bar textarea:focus { border-color: var(--blue-500); }
    .chat-send-btn { background: var(--blue-600); color: white; border: none; border-radius: 12px; width: 40px; height: 40px; display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; transition: var(--transition); }
    .chat-send-btn:hover { background: var(--blue-700); }
    .chat-send-btn svg { width: 17px; height: 17px; stroke: currentColor; fill: none; stroke-width: 2; }

    /* ── RESOURCES ── */
    .resources-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1rem; }
    body.phone-view .resources-grid { grid-template-columns: 1fr; }
    .resource-card { background: rgba(255,255,255,.88); backdrop-filter: blur(8px); border-radius: var(--radius-xl); box-shadow: var(--shadow); padding: 1rem 1.1rem; transition: var(--transition); border: 1px solid rgba(255,255,255,.7); cursor: pointer; }
    .resource-card:hover { box-shadow: var(--shadow-lg); transform: translateY(-1px); }
    .resource-inner { display: flex; gap: .85rem; }
    .resource-icon { width: 46px; height: 46px; background: var(--blue-100); border-radius: 12px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
    .resource-icon svg { width: 22px; height: 22px; stroke: var(--blue-600); fill: none; stroke-width: 1.75; }
    .resource-body { flex: 1; min-width: 0; }
    .resource-title-row { display: flex; align-items: flex-start; justify-content: space-between; gap: .5rem; }
    .resource-title { font-weight: 600; font-size: .96rem; color: var(--blue-900); }
    .resource-link svg { width: 17px; height: 17px; stroke: var(--blue-600); fill: none; stroke-width: 2; }
    .resource-desc { font-size: .83rem; color: rgba(30,58,138,.7); line-height: 1.5; margin-top: .45rem; }
    .resource-footer { display: flex; flex-wrap: wrap; gap: .38rem; margin-top: .55rem; align-items: center; }
    .resource-footer span { font-size: .72rem; color: rgba(37,99,235,.5); }
    .badge { display: inline-flex; align-items: center; padding: .18rem .58rem; border-radius: 99px; font-size: .7rem; font-weight: 600; }
    .badge-blue { background: var(--blue-100); color: var(--blue-800); }
    .badge-purple { background: #f3e8ff; color: #6b21a8; }
    .badge-green { background: #dcfce7; color: #166534; }
    .badge-orange { background: #ffedd5; color: #9a3412; }
    .badge-gray { background: #f3f4f6; color: #374151; }
    .badge-outline { background: transparent; border: 1px solid var(--blue-200); color: var(--blue-600); }

    /* ── MODALS ── */
    .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(15,30,80,.35); backdrop-filter: blur(4px); z-index: 200; align-items: center; justify-content: center; padding: 1rem; }
    .modal-overlay.open { display: flex; }
    .modal-box { background: white; border-radius: var(--radius-xl); box-shadow: 0 24px 60px rgba(0,0,0,.2); width: 100%; max-width: 400px; padding: 2rem; animation: slideIn .22s ease both; }
    .modal-title { font-family: 'DM Serif Display', serif; font-size: 1.35rem; color: var(--blue-900); margin-bottom: .3rem; }
    .modal-sub { font-size: .84rem; color: rgba(37,99,235,.6); margin-bottom: 1.2rem; }
    .modal-content { background: white; border-radius: var(--radius-xl); box-shadow: var(--shadow-xl); max-width: 500px; width: 90%; max-height: 90vh; overflow-y: auto; padding: 2rem; position: relative; }
    .modal-close { position: absolute; top: 1rem; right: 1rem; background: none; border: none; cursor: pointer; color: #999; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; }
    .modal-close:hover { color: var(--blue-600); }
    .modal-close svg { width: 20px; height: 20px; stroke: currentColor; fill: none; stroke-width: 2; }

    /* ── NOTIFICATIONS ── */
    .notif-header-row { display: flex; align-items: center; justify-content: space-between; }
    .notif-card { background: rgba(255,255,255,.88); backdrop-filter: blur(8px); border-radius: var(--radius-xl); box-shadow: var(--shadow); padding: .95rem 1rem; margin-bottom: .7rem; cursor: pointer; transition: var(--transition); border: 1px solid rgba(255,255,255,.7); }
    .notif-card:hover { box-shadow: var(--shadow-lg); }
    .notif-card.unread { background: rgba(219,234,254,.55); }
    .notif-inner { display: flex; gap: .8rem; }
    .notif-icon { width: 38px; height: 38px; border-radius: 50%; background: var(--blue-100); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
    .notif-card.unread .notif-icon { background: var(--blue-600); }
    .notif-icon svg { width: 16px; height: 16px; stroke: var(--blue-600); fill: none; stroke-width: 2; }
    .notif-card.unread .notif-icon svg { stroke: white; }
    .notif-body { flex: 1; min-width: 0; }
    .notif-row1 { display: flex; justify-content: space-between; align-items: flex-start; }
    .notif-title { font-weight: 600; color: var(--blue-900); font-size: .88rem; }
    .notif-date { font-size: .71rem; color: rgba(37,99,235,.5); }
    .notif-message { font-size: .82rem; color: rgba(37,99,235,.6); margin-top: .22rem; }
    .notif-from { font-size: .71rem; color: rgba(37,99,235,.45); margin-top: .3rem; }

    /* ── SETTINGS ── */
    .settings-card { background: rgba(255,255,255,.88); backdrop-filter: blur(8px); border-radius: var(--radius-xl); box-shadow: var(--shadow); margin-bottom: .9rem; overflow: hidden; border: 1px solid rgba(255,255,255,.7); }
    .settings-card-header { padding: 1rem 1.2rem .5rem; display: flex; align-items: center; gap: .55rem; }
    .settings-card-header svg { width: 18px; height: 18px; stroke: var(--blue-600); fill: none; stroke-width: 2; }
    .settings-card-header h2 { font-weight: 600; font-size: .95rem; color: var(--blue-900); }
    .settings-card-body { padding: 0 1.2rem 1.2rem; }
    .profile-top { display: flex; align-items: center; gap: .9rem; margin-bottom: 1.1rem; }
    .profile-top-info p { font-weight: 600; color: var(--blue-900); }
    .profile-top-info small { font-size: .78rem; color: rgba(37,99,235,.55); display: flex; align-items: center; gap: .2rem; }
    .profile-top-info small svg { width: 12px; height: 12px; stroke: currentColor; fill: none; stroke-width: 2; }
    .role-tag { font-size: .73rem; color: var(--blue-500); text-transform: capitalize; margin-top: .12rem; font-weight: 500; }
    .toggle-row { display: flex; align-items: center; justify-content: space-between; background: var(--blue-50); border-radius: var(--radius); padding: .9rem; margin-bottom: .75rem; }
    .toggle-row-text p { font-weight: 500; font-size: .88rem; color: var(--blue-900); }
    .toggle-row-text small { font-size: .76rem; color: rgba(37,99,235,.55); }
    .toggle { position: relative; display: inline-block; width: 42px; height: 23px; }
    .toggle input { opacity: 0; width: 0; height: 0; }
    .toggle-slider { position: absolute; cursor: pointer; inset: 0; background: #cbd5e1; border-radius: 12px; transition: .2s; }
    .toggle-slider::before { content: ''; position: absolute; width: 17px; height: 17px; background: white; border-radius: 50%; left: 3px; bottom: 3px; transition: .2s; box-shadow: 0 1px 4px rgba(0,0,0,.18); }
    .toggle input:checked + .toggle-slider { background: var(--blue-600); }
    .toggle input:checked + .toggle-slider::before { transform: translateX(19px); }
    .toggle input:disabled + .toggle-slider { opacity: .4; cursor: not-allowed; }
    .account-item { display: flex; align-items: center; justify-content: space-between; background: var(--blue-50); border-radius: var(--radius); padding: .9rem; margin-bottom: .7rem; }
    .account-item p { font-weight: 500; font-size: .88rem; color: var(--blue-900); }
    .account-item small { font-size: .76rem; color: rgba(37,99,235,.55); }
    .tabroom-status { display: flex; align-items: center; gap: .6rem; padding: .75rem 1rem; border-radius: var(--radius); margin-top: .4rem; font-size: .84rem; }
    .tabroom-status.linked { background: var(--green-50); border: 1px solid var(--green-200); color: var(--green-700); }
    .tabroom-status.unlinked { background: var(--blue-50); border: 1px solid var(--blue-200); color: var(--blue-700); }
    .tabroom-status svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 2; flex-shrink: 0; }

    /* ── MISC ── */
    .empty-state { text-align: center; padding: 3rem 1rem; }
    .empty-state svg { width: 48px; height: 48px; stroke: var(--blue-300); fill: none; stroke-width: 1.5; margin: 0 auto .9rem; display: block; }
    .empty-state p { color: rgba(37,99,235,.55); font-weight: 500; }
    .empty-state small { font-size: .82rem; color: rgba(37,99,235,.38); display: block; margin-top: .25rem; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .spinner { width: 18px; height: 18px; border: 2.5px solid rgba(255,255,255,.4); border-top-color: white; border-radius: 50%; animation: spin .7s linear infinite; display: inline-block; }
    .spinner-blue { border-color: var(--blue-200); border-top-color: var(--blue-600); }
    @keyframes fadeUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes slideIn { from { opacity: 0; transform: scale(.97) translateY(8px); } to { opacity: 1; transform: scale(1) translateY(0); } }
    .anim-slide { animation: slideIn .25s ease both; }
    .section-divider { display: flex; align-items: center; gap: .75rem; margin: .6rem 0; }
    .section-divider hr { flex: 1; border: none; border-top: 1px solid var(--blue-100); }
    .section-divider span { font-size: .76rem; color: rgba(37,99,235,.4); white-space: nowrap; font-weight: 500; }

    /* Keyboard hint */
    .keyboard-hint {
      position: fixed; bottom: 1rem; right: 1rem;
      background: rgba(255,255,255,.9); backdrop-filter: blur(8px);
      border: 1px solid var(--blue-100); border-radius: var(--radius);
      padding: .5rem .85rem; font-size: .75rem; color: rgba(37,99,235,.5);
      display: flex; align-items: center; gap: .4rem;
      box-shadow: var(--shadow-sm); z-index: 100;
      transition: opacity .3s;
    }
    .keyboard-hint kbd { background: var(--blue-50); border: 1px solid var(--blue-200); border-radius: 5px; padding: .1rem .35rem; font-size: .7rem; color: var(--blue-700); font-family: monospace; }
    body.phone-view .keyboard-hint { bottom: 70px; }
    /* Hide toggle hint on real mobile devices */
    @media (hover: none) and (pointer: coarse), (max-width: 600px) {
      .keyboard-hint { display: none !important; }
    }
    /* On real mobile (touch) devices OR small screens: always use bottom nav layout, never sidebar */
    @media (hover: none) and (pointer: coarse), (max-width: 600px) {
      .bottom-nav {
        position: fixed !important;
        left: 50% !important; transform: translateX(-50%) !important;
        top: auto !important; bottom: 0 !important;
        width: 100% !important; max-width: 100% !important;
        height: auto !important;
        border-right: none !important;
        border-top: 1px solid var(--blue-100) !important;
        flex-direction: row !important;
      }
      .bottom-nav-inner {
        flex-direction: row !important;
        justify-content: space-around !important;
        padding: .35rem .4rem !important;
        gap: 0 !important;
      }
      .sidebar-logo { display: none !important; }
      .nav-item {
        flex-direction: column !important;
        align-items: center !important;
        gap: .17rem !important;
        padding: .45rem .55rem !important;
        font-size: .67rem !important;
        min-width: 48px !important;
        width: auto !important;
      }
      .nav-badge { position: absolute !important; top: 1px !important; right: 2px !important; margin-left: 0 !important; }
      .app-layout .top-bar-inner { padding-left: 1.2rem !important; }
      .app-layout .page-content { padding-left: 1.2rem !important; padding-bottom: 6.5rem !important; }
      .person-grid { grid-template-columns: 1fr !important; }
      .resources-grid { grid-template-columns: 1fr !important; }
    }

    @media (max-width: 480px) {
      .auth-inner { max-width: 100%; }
      .card-header, .card-body { padding-left: 1.3rem; padding-right: 1.3rem; }
      .grid-2 { grid-template-columns: 1fr; }
      .verify-digit { width: 44px; height: 54px; font-size: 1.4rem; }
    }

    /* ── SETTINGS TABS + SWIPE ── */
    .settings-tab-bar {
      display: flex; gap: .35rem; padding: .6rem 1.2rem .7rem;
      overflow-x: auto; scrollbar-width: none; -ms-overflow-style: none;
      border-bottom: 1px solid var(--blue-100);
      background: rgba(255,255,255,.92); backdrop-filter: blur(14px);
    }
    .settings-tab-bar::-webkit-scrollbar { display: none; }
    .settings-tab {
      flex-shrink: 0; padding: .38rem .95rem; border-radius: 99px;
      border: 1.5px solid var(--blue-200); background: transparent;
      font-family: 'DM Sans', sans-serif; font-size: .82rem; font-weight: 600;
      color: rgba(37,99,235,.6); cursor: pointer; transition: all .18s;
    }
    .settings-tab.active { background: var(--blue-600); border-color: var(--blue-600); color: white; }
    .settings-tab:not(.active):hover { background: var(--blue-50); border-color: var(--blue-400); color: var(--blue-700); }
    .settings-track {
      display: flex; flex-direction: row;
      transition: transform .32s cubic-bezier(.4,0,.2,1);
      will-change: transform; touch-action: pan-y;
      align-items: flex-start;
    }
    .settings-panel {
      flex: 0 0 100%; width: 100%; min-width: 0;
      padding: 1.2rem 1.2rem 2rem;
      box-sizing: border-box;
    }

    /* ── NOTIFICATION ACTIONS ── */
    .notif-card { position: relative; }
    .notif-actions { display: flex; gap: .35rem; margin-left: auto; flex-shrink: 0; opacity: 0; transition: opacity .15s; }
    .notif-card:hover .notif-actions { opacity: 1; }
    @media (hover: none), (max-width: 600px) { .notif-actions { opacity: 1; } }
    .notif-action-btn {
      background: none; border: none; cursor: pointer; border-radius: 8px;
      width: 30px; height: 30px; display: flex; align-items: center; justify-content: center;
      color: rgba(37,99,235,.4); transition: all .15s;
    }
    .notif-action-btn:hover { background: var(--blue-50); color: var(--blue-600); }
    .notif-action-btn.delete:hover { background: var(--red-50); color: var(--red-600); }
    .notif-action-btn svg { width: 15px; height: 15px; stroke: currentColor; fill: none; stroke-width: 2; }
  </style>
</head>
<body>

<!-- LANDING -->
<div id="page-landing" class="page active">
  <div class="auth-wrap">
    <div class="auth-inner">
      <div class="logo-block">
        <div class="logo-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div>
        <h1 class="logo-title">Mentor Connect</h1>
        <p class="logo-sub">The mentoring community hub</p>
      </div>
      <div class="card anim-slide">
        <div class="card-body" style="display:flex;flex-direction:column;gap:.7rem;">
          <button class="btn btn-primary" onclick="showPage('login')">
            <svg viewBox="0 0 24 24"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><path d="M10 17 L15 12 L10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>Log In
          </button>
          <button class="btn btn-outline" onclick="showPage('signup-role')">
            <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/></svg>Create Account
          </button>
        </div>
      </div>
      <div class="role-grid">
        <div class="role-tile"><svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg><h3>Mentors</h3><p>Guide and teach</p></div>
        <div class="role-tile"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg><h3>Judges</h3><p>Evaluate debates</p></div>
        <div class="role-tile"><svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg><h3>Coaches</h3><p>Recruit &amp; build teams</p></div>
        <div class="role-tile"><svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg><h3>Teachers</h3><p>Educate students</p></div>
      </div>
    </div>
  </div>
</div>

<!-- LOGIN -->
<div id="page-login" class="page">
  <div class="auth-wrap"><div class="auth-inner">
    <div class="logo-block"><div class="logo-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div><h1 class="logo-title">Mentor Connect</h1></div>
    <div class="card anim-slide">
      <div class="card-header">
        <button class="btn-back" onclick="showPage('landing')"><svg viewBox="0 0 24 24"><path d="M15 18 L9 12 L15 6"/></svg>Back</button>
        <h2 class="card-title">Welcome back</h2><p class="card-sub">Sign in to your account</p>
      </div>
      <div class="card-body">
        <div id="login-error" class="alert alert-error" style="display:none"></div>
        <div class="form-group"><label class="form-label">Email</label><input id="login-email" type="email" class="form-input" placeholder="you@example.com" /></div>
        <div class="form-group"><label class="form-label">Password</label>
          <div class="input-wrap"><input id="login-password" type="password" class="form-input" placeholder="••••••••" style="padding-right:2.5rem" onkeydown="if(event.key==='Enter')handleLogin()" /><button class="input-eye" onclick="togglePwd('login-password',this)"><svg viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg></button></div>
        </div>
        <button id="login-btn" class="btn btn-primary" onclick="handleLogin()">Log In</button>
        <p class="divider-text">Don't have an account? <button onclick="showPage('signup-role')">Sign up</button></p>
      </div>
    </div>
  </div></div>
</div>

<!-- SIGNUP ROLE -->
<div id="page-signup-role" class="page">
  <div class="auth-wrap"><div class="auth-inner">
    <div class="logo-block"><div class="logo-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div><h1 class="logo-title">Mentor Connect</h1></div>
    <div class="card anim-slide">
      <div class="card-header">
        <button class="btn-back" onclick="showPage('landing')"><svg viewBox="0 0 24 24"><path d="M15 18 L9 12 L15 6"/></svg>Back</button>
        <h2 class="card-title">Create an account</h2><p class="card-sub">Step 1 of 2 — Choose your role</p>
      </div>
      <div class="card-body">
        <button class="role-option" data-role="mentor" onclick="selectRole('mentor')"><div class="role-option-icon"><svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg></div><div class="role-option-text"><h3>Mentor</h3><p>Guide and teach others in debate</p></div><div class="role-check"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg></div></button>
        <button class="role-option" data-role="judge" onclick="selectRole('judge')"><div class="role-option-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div><div class="role-option-text"><h3>Judge</h3><p>Evaluate debates and competitions</p></div><div class="role-check"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg></div></button>
        <button class="role-option" data-role="coach" onclick="selectRole('coach')"><div class="role-option-icon"><svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg></div><div class="role-option-text"><h3>Coach</h3><p>Coach your school's team &amp; recruit judges/mentors</p></div><div class="role-check"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg></div></button>
        <button class="role-option" data-role="teacher" onclick="selectRole('teacher')"><div class="role-option-icon"><svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg></div><div class="role-option-text"><h3>Teacher</h3><p>Educate students in schools</p></div><div class="role-check"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg></div></button>
        <button id="role-continue-btn" class="btn btn-primary" style="margin-top:.9rem;opacity:.45;pointer-events:none" onclick="goToSignupInfo()">Continue</button>
        <p class="divider-text">Already have an account? <button onclick="showPage('login')">Log in</button></p>
      </div>
    </div>
  </div></div>
</div>

<!-- SIGNUP INFO -->
<div id="page-signup-info" class="page">
  <div class="auth-wrap"><div class="auth-inner">
    <div class="logo-block"><div class="logo-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div><h1 class="logo-title">Mentor Connect</h1></div>
    <div class="card anim-slide">
      <div class="card-header">
        <button class="btn-back" onclick="showPage('signup-role')"><svg viewBox="0 0 24 24"><path d="M15 18 L9 12 L15 6"/></svg>Back</button>
        <h2 class="card-title">Your Information</h2>
        <p class="card-sub">Step 2 of 2 — Signing up as <span id="signup-role-label" style="font-weight:700;color:var(--blue-700);text-transform:capitalize"></span></p>
      </div>
      <div class="card-body">
        <div id="signup-error" class="alert alert-error" style="display:none"></div>
        <div class="grid-2">
          <div class="form-group"><label class="form-label">First Name *</label><input id="signup-first" type="text" class="form-input" /></div>
          <div class="form-group"><label class="form-label">Last Name *</label><input id="signup-last" type="text" class="form-input" /></div>
        </div>
        <div class="form-group"><label class="form-label">Email *</label><input id="signup-email" type="email" class="form-input" placeholder="you@example.com" /></div>
        <div class="form-group"><label class="form-label">Password *</label>
          <div class="input-wrap"><input id="signup-password" type="password" class="form-input" placeholder="••••••••" style="padding-right:2.5rem" /><button class="input-eye" onclick="togglePwd('signup-password',this)"><svg viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg></button></div>
        </div>
        <div class="form-group"><label class="form-label">Location * (City, State)</label>
          <div class="autocomplete-wrap"><input id="signup-location" type="text" class="form-input" placeholder="e.g. Los Angeles, CA" oninput="acSearch('signup-location','ac-loc-signup',LOCATIONS)" onkeydown="acKeydown(event,'ac-loc-signup','signup-location')" onblur="setTimeout(()=>closeAc('ac-loc-signup'),200)" /><div id="ac-loc-signup" class="autocomplete-dropdown"></div></div>
        </div>
        <div id="mentor-school-section" style="display:none">
          <div class="checkbox-wrap"><input type="checkbox" id="not-in-school" onchange="toggleSchoolField()" /><label for="not-in-school">I'm not currently in school</label></div>
          <div id="school-field" class="form-group"><label class="form-label">School *</label>
            <div class="autocomplete-wrap"><input id="signup-school-mentor" type="text" class="form-input" placeholder="Start typing your school…" oninput="acSearch('signup-school-mentor','ac-school-mentor',SCHOOLS)" onkeydown="acKeydown(event,'ac-school-mentor','signup-school-mentor')" onblur="setTimeout(()=>closeAc('ac-school-mentor'),200)" /><div id="ac-school-mentor" class="autocomplete-dropdown"></div></div>
          </div>
        </div>
        <div id="coach-teacher-school-section" style="display:none" class="form-group"><label class="form-label">School</label>
          <div class="autocomplete-wrap"><input id="signup-school-ct" type="text" class="form-input" placeholder="Start typing your school…" oninput="acSearch('signup-school-ct','ac-school-ct',SCHOOLS)" onkeydown="acKeydown(event,'ac-school-ct','signup-school-ct')" onblur="setTimeout(()=>closeAc('ac-school-ct'),200)" /><div id="ac-school-ct" class="autocomplete-dropdown"></div></div>
        </div>
        <div id="judge-tabroom-section" style="display:none">
          <div class="form-group"><label class="form-label">Tabroom Username (optional)</label><input id="signup-tabroom" type="text" class="form-input" placeholder="Your Tabroom.com username" /><p style="font-size:.75rem;color:rgba(37,99,235,.45);margin-top:.3rem">Link your Tabroom account to verify your judging experience</p></div>
        </div>
        <button id="signup-btn" class="btn btn-primary" onclick="handleSignup()">Create Account &amp; Verify Email</button>
      </div>
    </div>
  </div></div>
</div>

<!-- VERIFY EMAIL -->
<div id="page-verify" class="page">
  <div class="auth-wrap"><div class="auth-inner">
    <div class="logo-block"><div class="logo-icon"><svg viewBox="0 0 24 24"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><path d="M22 6 L12 13 L2 6"/></svg></div><h1 class="logo-title">Verify Email</h1><p class="logo-sub" id="verify-sub">Check your inbox</p></div>
    <div class="card anim-slide">
      <div class="card-body">
        <div class="verify-hint" id="verify-hint" style="display:none"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg><span>Since this is a demo, your 6-digit code is shown below:</span></div>
        <div class="verify-code-display" id="verify-code-display" style="display:none">——————</div>
        <p style="font-size:.83rem;color:rgba(37,99,235,.6);text-align:center;margin-bottom:.5rem">Enter the code to verify your account</p>
        <div class="verify-digits" id="verify-digits">
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,0)" onkeydown="digitKeydown(event,this,0)" />
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,1)" onkeydown="digitKeydown(event,this,1)" />
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,2)" onkeydown="digitKeydown(event,this,2)" />
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,3)" onkeydown="digitKeydown(event,this,3)" />
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,4)" onkeydown="digitKeydown(event,this,4)" />
          <input class="verify-digit" maxlength="1" type="text" inputmode="numeric" pattern="[0-9]" oninput="digitInput(this,5)" onkeydown="digitKeydown(event,this,5)" />
        </div>
        <div id="verify-error" class="alert alert-error" style="display:none"></div>
        <button class="btn btn-primary" onclick="verifyCode()">Verify & Continue</button>
        <p class="divider-text" style="margin-top:.75rem"><button onclick="resendCode()">Resend code</button></p>
      </div>
    </div>
  </div></div>
</div>

<!-- MENTORS -->
<div id="page-mentors" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner">
      <div class="top-bar-title"><svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg><h1>Mentors</h1></div>
      <div style="display:flex;gap:.6rem;margin-top:.75rem;flex-wrap:wrap;align-items:center">
        <div class="search-bar-wrap" style="flex:1;min-width:180px;margin-bottom:0"><div class="search-icon"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg></div><input class="form-input" id="mentor-search" type="text" placeholder="Search mentors…" oninput="filterMentors()" /></div>
        <select class="form-select" id="mentor-location-filter" style="width:auto;flex:0 0 auto" onchange="filterMentors()"><option value="">All Locations</option></select>
        <select class="form-select" id="mentor-school-filter" style="width:auto;flex:0 0 auto" onchange="filterMentors()"><option value="">All Schools</option></select>
      </div>
    </div></div>
    <div class="page-content"><div class="page-content-inner" id="mentors-list"></div></div>
    <nav class="bottom-nav" id="bottom-nav-mentors"></nav>
  </div>
</div>

<!-- JUDGES -->
<div id="page-judges" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner">
      <div class="top-bar-title"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg><h1>Judges</h1></div>
      <div class="search-bar-wrap" style="margin-top:.75rem"><div class="search-icon"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg></div><input class="form-input" id="judge-search" type="text" placeholder="Search judges…" oninput="filterJudges()" /></div>
    </div></div>
    <div class="page-content"><div class="page-content-inner" id="judges-list"></div></div>
    <nav class="bottom-nav" id="bottom-nav-judges"></nav>
  </div>
</div>

<!-- MESSAGES -->
<div id="page-messages" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner"><div class="top-bar-title"><svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg><h1>Messages</h1></div></div></div>
    <div class="page-content"><div class="page-content-inner" style="max-width:700px" id="conversations-list"></div></div>
    <nav class="bottom-nav" id="bottom-nav-messages"></nav>
  </div>
</div>

<!-- CHAT -->
<div id="page-chat" class="page">
  <div class="chat-wrap">
    <div class="chat-header">
      <button class="btn-back" onclick="leaveChatPage()" style="margin-bottom:0"><svg viewBox="0 0 24 24"><path d="M15 18 L9 12 L15 6"/></svg></button>
      <div class="avatar avatar-sm" id="chat-avatar"></div>
      <div class="chat-header-info"><h2 id="chat-name"></h2><p id="chat-role"></p></div>
    </div>
    <div class="chat-messages" id="chat-messages-container"></div>
    <div class="chat-input-bar">
      <textarea id="chat-input" placeholder="Type a message…" rows="1" onkeydown="chatKeydown(event)"></textarea>
      <button class="chat-send-btn" onclick="sendMessage()"><svg viewBox="0 0 24 24"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg></button>
    </div>
  </div>
</div>

<!-- RESOURCES -->
<div id="page-resources" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner">
      <div class="top-bar-title"><svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg><h1>Resources</h1></div>
      <div style="display:flex;gap:.6rem;margin-top:.75rem;align-items:center;flex-wrap:wrap">
        <select class="form-select" id="resource-category-filter" style="width:auto;flex:0 0 auto" onchange="filterResources()"><option value="">All Categories</option><option value="debate">Debate</option><option value="public_speaking">Public Speaking</option><option value="coaching">Coaching</option><option value="judging">Judging</option><option value="general">General</option></select>
        <button class="btn btn-primary" id="admin-create-resource-btn" style="display:none;width:auto" onclick="showCreateResourceModal()"><svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>Create Resource</button>
      </div>
    </div></div>
    <div class="page-content"><div class="page-content-inner" id="resources-list"></div></div>
    <nav class="bottom-nav" id="bottom-nav-resources"></nav>
  </div>
</div>

<!-- RESOURCE DETAIL -->
<div id="page-resource-detail" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner"><button class="btn-back" onclick="showPage('resources')"><svg viewBox="0 0 24 24"><path d="M15 18 L9 12 L15 6"/></svg>Back</button></div></div>
    <div class="page-content" id="resource-detail-content"></div>
    <nav class="bottom-nav" id="bottom-nav-resource-detail"></nav>
  </div>
</div>

<!-- NOTIFICATIONS -->
<div id="page-notifications" class="page">
  <div class="app-layout">
    <div class="top-bar"><div class="top-bar-inner">
      <div class="notif-header-row">
        <div class="top-bar-title"><svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg><h1>Notifications</h1><span id="notif-unread-badge" class="badge badge-blue" style="display:none"></span></div>
        <button id="mark-all-btn" class="btn btn-ghost btn-sm" onclick="markAllRead()" style="display:none">Mark all</button>
      </div>
    </div></div>
    <div class="page-content"><div class="page-content-inner" style="max-width:700px" id="notifications-list"></div></div>
    <nav class="bottom-nav" id="bottom-nav-notifs"></nav>
  </div>
</div>

<!-- SETTINGS -->
<div id="page-settings" class="page">
  <div class="app-layout">
    <div class="top-bar">
      <div class="top-bar-inner">
        <div class="top-bar-title"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.68a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg><h1>Settings</h1></div>
      </div>
      <!-- Tab pills row -->
      <div class="settings-tab-bar" id="settings-tab-bar">
        <button class="settings-tab active" onclick="switchSettingsTab(0)" id="stab-0">Profile</button>
        <button class="settings-tab" onclick="switchSettingsTab(1)" id="stab-1">Account</button>
        <button class="settings-tab" onclick="switchSettingsTab(2)" id="stab-2" style="display:none">Admin</button>
      </div>
    </div>
    <div class="page-content" style="padding-top:0;overflow:hidden">
      <div id="settings-success" class="alert alert-success" style="display:none;margin:1rem 1rem 0"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg><span>Profile updated successfully!</span></div>
      <div id="settings-error" class="alert alert-error" style="display:none;margin:1rem 1rem 0"></div>

      <!-- Swipeable track -->
      <div class="settings-track" id="settings-track">

        <!-- Panel 0: Profile -->
        <div class="settings-panel" id="spanel-0">
          <div class="settings-card">
            <div class="settings-card-header"><svg viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg><h2>Profile Information</h2></div>
            <div class="settings-card-body">
              <div class="profile-top"><div class="avatar avatar-lg" id="settings-avatar"></div><div class="profile-top-info"><p id="settings-name"></p><small><svg viewBox="0 0 24 24"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><path d="M22 6 L12 13 L2 6"/></svg><span id="settings-email"></span></small><div class="role-tag" id="settings-role-tag"></div></div></div>
              <div class="grid-2"><div class="form-group"><label class="form-label">First Name</label><input id="settings-first" type="text" class="form-input" /></div><div class="form-group"><label class="form-label">Last Name</label><input id="settings-last" type="text" class="form-input" /></div></div>
              <div class="form-group"><label class="form-label">Location</label><div class="autocomplete-wrap"><input id="settings-location" type="text" class="form-input" oninput="acSearch('settings-location','ac-loc-settings',LOCATIONS)" onkeydown="acKeydown(event,'ac-loc-settings','settings-location')" onblur="setTimeout(()=>closeAc('ac-loc-settings'),200)" /><div id="ac-loc-settings" class="autocomplete-dropdown"></div></div></div>
              <div class="form-group" id="settings-school-wrap" style="display:none"><label class="form-label">School</label><div class="autocomplete-wrap"><input id="settings-school" type="text" class="form-input" oninput="acSearch('settings-school','ac-school-settings',SCHOOLS)" onkeydown="acKeydown(event,'ac-school-settings','settings-school')" onblur="setTimeout(()=>closeAc('ac-school-settings'),200)" /><div id="ac-school-settings" class="autocomplete-dropdown"></div></div></div>
              <div class="form-group"><label class="form-label">About You</label><textarea id="settings-bio" class="form-textarea"></textarea></div>
              <div class="toggle-row" id="settings-avail-row"><div class="toggle-row-text"><p>Available for hire</p><small>Let others know you're available</small></div><label class="toggle"><input type="checkbox" id="settings-available" /><span class="toggle-slider"></span></label></div>
              <div id="settings-tabroom-section" style="display:none">
                <div class="section-divider"><hr/><span>Tabroom Integration</span><hr/></div>
                <div class="form-group"><label class="form-label">Tabroom Username</label><input id="settings-tabroom" type="text" class="form-input" placeholder="Your Tabroom.com username" /></div>
                <div id="settings-tabroom-status"></div>
                <button class="btn btn-outline btn-sm" style="margin-bottom:.9rem;width:auto" onclick="linkTabroom()"><svg viewBox="0 0 24 24"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/></svg>Link / Update Tabroom</button>
              </div>
              <button class="btn btn-primary" onclick="saveSettings()"><svg viewBox="0 0 24 24"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><path d="M17 21 L17 13 L7 13 L7 21"/><path d="M7 3 L7 8 L15 8"/></svg>Save Changes</button>
            </div>
          </div>
        </div>

        <!-- Panel 1: Account -->
        <div class="settings-panel" id="spanel-1">
          <div class="settings-card">
            <div class="settings-card-header"><svg viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg><h2>Account</h2></div>
            <div class="settings-card-body">
              <div class="account-item"><div><p>Email Verified</p><small id="settings-email-sub"></small></div><svg id="settings-verify-icon" style="width:20px;height:20px;stroke:#16a34a;fill:none;stroke-width:2.5" viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12" stroke="#16a34a" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" fill="none"/></svg></div>
              <button class="btn btn-danger-outline" onclick="handleLogout()"><svg viewBox="0 0 24 24"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><path d="M16 17 L21 12 L16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>Log Out</button>
              <button class="btn btn-danger-outline" onclick="promptDeleteAccount()" style="margin-top:.6rem"><svg viewBox="0 0 24 24"><path d="M3 6 L5 6 L21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></svg>Delete Account</button>
            </div>
          </div>
        </div>

        <!-- Panel 2: Admin -->
        <div class="settings-panel" id="spanel-2">
          <div class="settings-card" id="admin-panel" style="display:block">
            <div class="settings-card-header"><svg viewBox="0 0 24 24"><path d="M12 2c6.627 0 12 5.373 12 12s-5.373 12-12 12S0 20.627 0 14 5.373 2 12 2z"/></svg><h2>Admin Controls</h2></div>
            <div class="settings-card-body">
              <p style="margin-bottom:1rem;color:#666;font-size:.9rem">Manage users and roles</p>
              <div class="form-group"><input type="text" class="form-input" id="admin-search-users" placeholder="Search by name or email..." oninput="filterAdminUsers()" /></div>
              <div id="admin-users-list" style="max-height:400px;overflow-y:auto"></div>
            </div>
          </div>
        </div>

      </div><!-- end track -->
    </div>
    <nav class="bottom-nav" id="bottom-nav-settings"></nav>
  </div>
</div>

<!-- CREATE RESOURCE MODAL -->
<div id="create-resource-modal" class="modal-overlay" onclick="if(event.target===this) closeCreateResourceModal()">
  <div class="modal-content">
    <button class="modal-close" onclick="closeCreateResourceModal()"><svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg></button>
    <h2 style="margin-bottom:1.5rem;color:var(--blue-900)">Create New Resource</h2>
    <div class="form-group"><label class="form-label">Title *</label><input id="new-res-title" type="text" class="form-input" placeholder="Enter resource title" /></div>
    <div class="form-group"><label class="form-label">Description / Content</label><textarea id="new-res-desc" class="form-textarea" placeholder="Write detailed content here..."></textarea></div>
    <div class="form-group"><label class="form-label">Link / URL</label><input id="new-res-url" type="text" class="form-input" placeholder="https://" /></div>
    <div class="grid-2">
      <div class="form-group"><label class="form-label">Category *</label><select id="new-res-cat" class="form-select"><option value="">Select category</option><option value="debate">Debate</option><option value="public_speaking">Public Speaking</option><option value="coaching">Coaching</option><option value="judging">Judging</option><option value="general">General</option></select></div>
      <div class="form-group"><label class="form-label">Type *</label><select id="new-res-type" class="form-select"><option value="">Select type</option><option value="document">Document</option><option value="video">Video</option><option value="link">Link</option><option value="default">Other</option></select></div>
    </div>
    <div style="display:flex;gap:.6rem;margin-top:1.5rem"><button class="btn btn-primary" onclick="createNewResource()">Create Resource</button><button class="btn btn-ghost" onclick="closeCreateResourceModal()">Cancel</button></div>
  </div>
</div>

<!-- KEYBOARD HINT -->
<div class="keyboard-hint" id="kb-hint">
  <svg style="width:14px;height:14px;stroke:currentColor;fill:none;stroke-width:2" viewBox="0 0 24 24"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M6 8h.01M10 8h.01M14 8h.01M18 8h.01M8 12h.01M12 12h.01M16 12h.01M7 16h10"/></svg>
  <span id="kb-hint-text"><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> — toggle phone view</span>
</div>

<script>
'use strict';

// ════ SUPABASE CONFIG ════
const SUPABASE_URL = 'https://knkclotptwudbqdwjqxp.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Imtua2Nsb3RwdHd1ZGJxZHdqcXhwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzIyOTQ1NjUsImV4cCI6MjA4Nzg3MDU2NX0.dk9OlP3DMP_Rw5cvxvlUYGEuEngnerdpNXu9iNI9ibc';
let supa = null;

function initSupabase() {
  if (!window.supabase) { console.error('Supabase SDK not loaded'); return; }
  try {
    supa = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
    // Run a quick write test after a short delay to catch RLS issues early
    setTimeout(testSupabaseWrite, 2000);
  }
  catch(e) { console.error('Supabase init failed', e); }
}

async function testSupabaseWrite() {
  if (!supa) return;
  const testId = 'test_' + Date.now();

  // Test 1: notifications
  const { error: nErr } = await supa.from('notifications').insert({
    id: testId, user_email: 'test@test.com', type: 'test',
    title: 'test', message: 'test', read: false, created_date: new Date().toISOString()
  });
  if (nErr) {
    console.error('notifications write failed:', nErr.code, nErr.message);
    showSupabaseWarning('notifications');
    return;
  }
  await supa.from('notifications').delete().eq('id', testId);

  // Test 2: conversations (most likely to fail due to array/jsonb column types)
  const testConvoId = 'test_convo_' + Date.now();
  const { error: cErr } = await supa.from('conversations').insert({
    id: testConvoId,
    participants: ['test@a.com', 'test@b.com'],
    participant_names: JSON.stringify({'test@a.com': 'Test A', 'test@b.com': 'Test B'}),
    last_message: null,
    last_message_date: new Date().toISOString(),
    unread_by: [],
    hidden_by: []
  });
  if (cErr) {
    console.error('conversations write failed:', cErr.code, cErr.message, cErr.details);
    showSupabaseWarning('conversations');
    return;
  }
  await supa.from('conversations').delete().eq('id', testConvoId);

  console.log('%c✅ Supabase write tests passed', 'color:green;font-weight:bold');
}

function showSupabaseWarning(table) {
  const existing = document.getElementById('supa-rls-warning');
  if (existing) return;
  const banner = document.createElement('div');
  banner.id = 'supa-rls-warning';
  banner.style.cssText = 'position:fixed;top:0;left:0;right:0;z-index:9999;background:#fef2f2;border-bottom:2px solid #dc2626;padding:.75rem 1rem;display:flex;align-items:center;gap:.75rem;font-size:.83rem;color:#7f1d1d;font-family:DM Sans,sans-serif;';
  const msg = table === 'conversations'
    ? `<strong>Supabase conversations table needs to be recreated</strong> with correct column types.`
    : `<strong>Supabase setup needed:</strong> Data won't save until you run the fix SQL.`;
  banner.innerHTML = `
    <svg style="width:18px;height:18px;stroke:#dc2626;fill:none;stroke-width:2;flex-shrink:0" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
    <span>${msg} <a href="#" onclick="showRlsInstructions();return false;" style="color:#dc2626;font-weight:600;text-decoration:underline">See SQL to fix</a></span>
    <button onclick="this.parentElement.remove()" style="margin-left:auto;background:none;border:none;cursor:pointer;color:#dc2626;font-size:1.1rem;padding:0 .3rem;">×</button>
  `;
  document.body.prepend(banner);
}

function showRlsInstructions() {
  const sql = `-- Run this in Supabase → SQL Editor → New Query
-- This fixes RLS AND recreates tables with correct column types

alter table if exists notifications disable row level security;
alter table if exists messages disable row level security;
alter table if exists users disable row level security;
alter table if exists resources disable row level security;

-- Drop and recreate conversations with correct types
drop table if exists conversations cascade;
create table conversations (
  id text primary key,
  participants text[],
  participant_names jsonb,
  last_message text,
  last_message_date timestamptz,
  unread_by text[],
  hidden_by text[]
);
alter table conversations disable row level security;

-- Drop and recreate messages in case of issues
drop table if exists messages cascade;
create table messages (
  id text primary key,
  convo_id text,
  from_email text,
  text text,
  created_date timestamptz
);
alter table messages disable row level security;

-- Make sure notifications table exists with right schema
create table if not exists notifications (
  id text primary key,
  user_email text,
  type text,
  title text,
  message text,
  reference_id text,
  from_user_name text,
  read boolean default false,
  created_date timestamptz
);
alter table notifications disable row level security;`;
  const modal = document.createElement('div');
  modal.style.cssText = 'position:fixed;inset:0;z-index:10000;background:rgba(0,0,0,.5);display:flex;align-items:center;justify-content:center;padding:1rem;';
  modal.innerHTML = `
    <div style="background:white;border-radius:16px;padding:1.75rem;max-width:520px;width:100%;box-shadow:0 24px 60px rgba(0,0,0,.2);">
      <h2 style="font-family:DM Serif Display,serif;font-size:1.3rem;color:#1e3a8a;margin-bottom:.5rem">Fix Supabase Permissions</h2>
      <p style="font-size:.84rem;color:#3730a3;margin-bottom:1rem">Go to <strong>supabase.com → your project → SQL Editor → New Query</strong>, paste this and click Run:</p>
      <pre style="background:#f1f5f9;border-radius:10px;padding:1rem;font-size:.78rem;overflow-x:auto;white-space:pre-wrap;border:1px solid #e2e8f0;color:#1e293b;">${sql}</pre>
      <div style="display:flex;gap:.6rem;margin-top:1.2rem">
        <button onclick="navigator.clipboard?.writeText(document.querySelector('pre').textContent);this.textContent='Copied!';setTimeout(()=>this.textContent='Copy SQL',2000)" style="flex:1;padding:.65rem;background:#2563eb;color:white;border:none;border-radius:10px;font-weight:600;cursor:pointer;font-size:.88rem;">Copy SQL</button>
        <button onclick="this.closest('div[style]').remove()" style="flex:1;padding:.65rem;background:#f1f5f9;color:#1e3a8a;border:none;border-radius:10px;font-weight:600;cursor:pointer;font-size:.88rem;">Close</button>
      </div>
    </div>
  `;
  modal.addEventListener('click', e => { if (e.target === modal) modal.remove(); });
  document.body.appendChild(modal);
}

// ════ PHONE VIEW TOGGLE ════
// Detect real touch/mobile device — don't show toggle on these
const IS_REAL_MOBILE = ('ontouchstart' in window) || (navigator.maxTouchPoints > 0) || window.innerWidth <= 600;

function updateKbHint() {
  const hint = document.getElementById('kb-hint');
  const hintText = document.getElementById('kb-hint-text');
  if (!hint) return;
  if (IS_REAL_MOBILE) { hint.style.display = 'none'; return; }
  const isPhone = document.body.classList.contains('phone-view');
  hintText.innerHTML = isPhone
    ? `<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> — toggle computer view`
    : `<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> — toggle phone view`;
}

document.addEventListener('keydown', (e) => {
  if (e.ctrlKey && e.shiftKey && e.key === 'E') {
    if (IS_REAL_MOBILE) return; // No toggle on real mobile
    e.preventDefault();
    document.body.classList.toggle('phone-view');
    const hint = document.getElementById('kb-hint');
    if (hint) {
      hint.style.opacity = '0';
      setTimeout(() => { hint.style.opacity = '1'; updateKbHint(); }, 300);
    }
    // Rebuild navs so sidebar/bottom bar switches correctly
    renderAllNavs();
  }
});

// ════ AUTOCOMPLETE DATA ════
const LOCATIONS = [
  'Alabama','Alaska','Arizona','Arkansas','California','Colorado',
  'Connecticut','Delaware','Florida','Georgia','Hawaii','Idaho',
  'Illinois','Indiana','Iowa','Kansas','Kentucky','Louisiana',
  'Maine','Maryland','Massachusetts','Michigan','Minnesota',
  'Mississippi','Missouri','Montana','Nebraska','Nevada',
  'New Hampshire','New Jersey','New Mexico','New York',
  'North Carolina','North Dakota','Ohio','Oklahoma','Oregon',
  'Pennsylvania','Rhode Island','South Carolina','South Dakota',
  'Tennessee','Texas','Utah','Vermont','Virginia','Washington',
  'West Virginia','Wisconsin','Wyoming',
  'Afghanistan','Albania','Algeria','Andorra','Angola','Antigua and Barbuda',
  'Argentina','Armenia','Australia','Austria','Azerbaijan','Bahamas',
  'Bahrain','Bangladesh','Barbados','Belarus','Belgium','Belize',
  'Benin','Bhutan','Bolivia','Bosnia and Herzegovina','Botswana',
  'Brazil','Brunei','Bulgaria','Burkina Faso','Burundi','Cabo Verde',
  'Cambodia','Cameroon','Canada','Central African Republic','Chad',
  'Chile','China','Colombia','Comoros','Congo (Congo-Brazzaville)',
  'Costa Rica','Croatia','Cuba','Cyprus','Czechia','Denmark',
  'Djibouti','Dominica','Dominican Republic','Ecuador','Egypt',
  'El Salvador','Equatorial Guinea','Eritrea','Estonia','Eswatini',
  'Ethiopia','Fiji','Finland','France','Gabon','Gambia',
  'Georgia','Germany','Ghana','Greece','Grenada','Guatemala',
  'Guinea','Guinea-Bissau','Guyana','Haiti','Honduras',
  'Hungary','Iceland','India','Indonesia','Iran','Iraq',
  'Ireland','Israel','Italy','Jamaica','Japan','Jordan',
  'Kazakhstan','Kenya','Kiribati','Kuwait','Kyrgyzstan',
  'Laos','Latvia','Lebanon','Lesotho','Liberia','Libya',
  'Liechtenstein','Lithuania','Luxembourg','Madagascar','Malawi',
  'Malaysia','Maldives','Mali','Malta','Marshall Islands',
  'Mauritania','Mauritius','Mexico','Micronesia','Moldova',
  'Monaco','Mongolia','Montenegro','Morocco','Mozambique',
  'Myanmar','Namibia','Nauru','Nepal','Netherlands',
  'New Zealand','Nicaragua','Niger','Nigeria','North Korea',
  'North Macedonia','Norway','Oman','Pakistan','Palau',
  'Panama','Papua New Guinea','Paraguay','Peru','Philippines',
  'Poland','Portugal','Qatar','Romania','Russia',
  'Rwanda','Saint Kitts and Nevis','Saint Lucia',
  'Saint Vincent and the Grenadines','Samoa','San Marino',
  'Sao Tome and Principe','Saudi Arabia','Senegal','Serbia',
  'Seychelles','Sierra Leone','Singapore','Slovakia','Slovenia',
  'Solomon Islands','Somalia','South Africa','South Korea',
  'South Sudan','Spain','Sri Lanka','Sudan','Suriname',
  'Sweden','Switzerland','Syria','Taiwan','Tajikistan',
  'Tanzania','Thailand','Timor-Leste','Togo','Tonga',
  'Trinidad and Tobago','Tunisia','Turkey','Turkmenistan',
  'Tuvalu','Uganda','Ukraine','United Arab Emirates',
  'United Kingdom','United States','Uruguay','Uzbekistan',
  'Vanuatu','Vatican City','Venezuela','Vietnam','Yemen','Zambia','Zimbabwe'];

const SCHOOLS = [
  'Auburn University','Boston College','Boston University','Brown University',
  'California Institute of Technology','Carnegie Mellon University','Columbia University',
  'Cornell University','Dartmouth College','Duke University','Emory University',
  'Florida State University','Georgetown University','Georgia Institute of Technology',
  'Harvard University','Indiana University','Johns Hopkins University',
  'Massachusetts Institute of Technology','Michigan State University','New York University',
  'Northwestern University','Ohio State University','Penn State University',
  'Princeton University','Purdue University','Rice University','Stanford University',
  'Tufts University','Tulane University','UC Berkeley','UCLA','UC San Diego',
  'University of Arizona','University of Chicago','University of Colorado',
  'University of Florida','University of Georgia','University of Illinois',
  'University of Maryland','University of Michigan','University of Minnesota',
  'University of North Carolina','University of Notre Dame','University of Pennsylvania',
  'University of Southern California','University of Texas','University of Virginia',
  'University of Washington','Vanderbilt University','Wake Forest University',
  'Washington University in St. Louis','Yale University','University of Oxford',
  'University of Cambridge','Imperial College London','London School of Economics',
  'University of Toronto','McGill University','University of British Columbia',
  'Australian National University','University of Melbourne','University of Sydney',
  'National University of Singapore','University of Hong Kong','Tsinghua University',
  'Peking University','University of Tokyo','Seoul National University','ETH Zurich',
  'University of Amsterdam','Sorbonne University','Heidelberg University',
  'University of Copenhagen','University of Barcelona','University of Cape Town',
  'Kings College London','Monash University','University of Auckland',
  'Bronx Science','Brooklyn Technical High School','Exeter Academy',
  'Harvard-Westlake School','Horace Mann School','Interlachen Arts Academy',
  'Lakeside School','Lexington High School','Loyola High School',
  'Middlesex School','Milton Academy','North Carolina School of Science',
  'Northside College Prep','Palo Alto High School','Phillips Academy Andover',
  'Regis High School','Ridgewood High School','Stuyvesant High School',
  'The Harker School','Thomas Jefferson High School','Trinity School',
  'Westlake High School','Whitney Young High School','Forest Hills High School',
  'Fiorello H. LaGuardia High School','Chelsea High School','Lincoln Park High School',
  'Madison High School','Roosevelt High School','Kennedy High School','Adams High School',
  'Eton College','Harrow School','Geelong Grammar School','St. Pauls School',
  'Raffles Institution','UWC Atlantic College','United World College of South East Asia',
  'International School of Geneva High School','Singapore American School High School',
  'Halsey Middle School','Lincoln Middle School','Madison Middle School',
  'Jefferson Davis Middle School','Central Middle School','Roosevelt Junior High',
  'Kennedy Junior High','Adams Junior High','Bronx Middle School of Science',
  'Brooklyn Latin Middle School','The Harker Middle School',
  'Thomas Jefferson Middle School','Trinity Middle School','Westlake Middle School',
  'Whitney Young Middle School','Springfield Middle School'];

// ════ STATE ════
let state = {
  currentUser: null,
  selectedRole: null,
  pendingUser: null,
  verifyCode: null,
  users: [],
  conversations: [],
  notifications: [],
  resources: [],
  messages: {},
  activeConvoId: null,
  currentResourceId: null,
  pollInterval: null,
};

const DEMO_RESOURCES = [
  { id:'r1', title:'Introduction to Lincoln-Douglas Debate', type:'document', category:'debate', description:'A comprehensive guide to LD debate format, including value/criterion framework construction.', url:'#', posted_by_name:'Mentor Connect Team', created_date:'2024-11-15T00:00:00Z' },
  { id:'r2', title:'How to Research Evidence Effectively', type:'video', category:'debate', description:'Learn how to find and cut cards efficiently for policy debate.', url:'#', posted_by_name:'Mentor Connect Team', created_date:'2024-12-01T00:00:00Z' },
  { id:'r3', title:'Public Speaking Fundamentals', type:'link', category:'public_speaking', description:'Tips for improving your delivery, eye contact, and vocal variety on the debate floor.', url:'#', posted_by_name:'Mentor Connect Team', created_date:'2025-01-10T00:00:00Z' },
  { id:'r4', title:'Judging Philosophy Examples', type:'document', category:'judging', description:'Sample judging philosophies from experienced coaches and judges.', url:'#', posted_by_name:'Mentor Connect Team', created_date:'2025-02-01T00:00:00Z' },
];

const CAT_BADGE = { debate:'badge-blue', public_speaking:'badge-purple', coaching:'badge-green', judging:'badge-orange', general:'badge-gray' };
const TYPE_ICONS = {
  video:    `<svg viewBox="0 0 24 24"><polygon points="23 7 16 12 23 17 23 7"/><rect x="1" y="5" width="15" height="14" rx="2"/></svg>`,
  document: `<svg viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><path d="M14 2 L14 8 L20 8"/></svg>`,
  link:     `<svg viewBox="0 0 24 24"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/></svg>`,
  default:  `<svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>`
};

const NAV_ITEMS = [
  { id:'mentors',       label:'Mentors',       page:'mentors',       svg:`<svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>` },
  { id:'judges',        label:'Judges',        page:'judges',        svg:`<svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg>` },
  { id:'messages',      label:'Messages',      page:'messages',      svg:`<svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>` },
  { id:'notifications', label:'Notifications',        page:'notifications', svg:`<svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>` },
  { id:'resources',     label:'Resources',     page:'resources',     svg:`<svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>` },
  { id:'settings',      label:'Settings',      page:'settings',      svg:`<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.68a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>` },
];

// ════ SUPABASE CRUD ════
async function pushUser(user) {
  if (!supa) return;
  try {
    const { error } = await supa.from('users').upsert({
      id: user.id, email: user.email, password: user.password,
      first_name: user.first_name, last_name: user.last_name,
      role: user.role, location: user.location, school: user.school || null,
      description: user.description || null, available_for_hire: user.available_for_hire,
      tabroom_username: user.tabroom_username || null, tabroom_linked: user.tabroom_linked || false,
      email_verified: user.email_verified
    }, { onConflict: 'id' });
    if (error) console.error('pushUser error:', error.message);
  } catch(e) { console.error('pushUser exception:', e); }
}

async function deleteUserFromSupabase(userId) {
  if (!supa) return;
  try {
    const userEmail = state.users.find(u=>u.id===userId)?.email;
    if (userEmail) {
      await supa.from('messages').delete().eq('from_email', userEmail);
      const { data: convos } = await supa.from('conversations').select('id').contains('participants', [userEmail]);
      if (convos && convos.length) {
        for (const c of convos) await supa.from('messages').delete().eq('convo_id', c.id);
        await supa.from('conversations').delete().contains('participants', [userEmail]);
      }
    }
    const { error } = await supa.from('users').delete().eq('id', userId);
    if (error) console.error('deleteUser error:', error.message);
  } catch(e) { console.error('deleteUserFromSupabase exception:', e); }
}

async function pushResource(res) {
  if (!supa) return;
  try {
    const { error } = await supa.from('resources').upsert({
      id: res.id, title: res.title, description: res.description || null,
      url: res.url || null, category: res.category, type: res.type,
      posted_by_name: res.posted_by_name || null,
      created_date: res.created_date || new Date().toISOString(),
      updated_date: res.updated_date || new Date().toISOString()
    }, { onConflict: 'id' });
    if (error) console.error('pushResource error:', error.message);
  } catch(e) { console.error('pushResource exception:', e); }
}

async function deleteResourceFromSupabase(resourceId) {
  if (!supa) return;
  try {
    const { error } = await supa.from('resources').delete().eq('id', resourceId);
    if (error) console.error('deleteResource error:', error.message);
  } catch(e) { console.error('deleteResourceFromSupabase exception:', e); }
}

async function pushConversation(convo) {
  if (!supa) return;
  try {
    const payload = {
      id: convo.id,
      participants: convo.participants,
      // Supabase jsonb columns accept plain objects — no need to stringify
      participant_names: convo.participant_names || {},
      last_message: convo.last_message || null,
      last_message_date: convo.last_message_date || new Date().toISOString(),
      unread_by: convo.unread_by || [],
      hidden_by: convo.hidden_by || []
    };
    console.log('pushConversation payload:', JSON.stringify(payload));
    const { error } = await supa.from('conversations').upsert(payload, { onConflict: 'id' });
    if (error) {
      console.warn('pushConversation upsert error:', error.code, error.message, error.details);
      // fallback: try plain insert
      const { error: insErr } = await supa.from('conversations').insert(payload);
      if (insErr) {
        console.error('pushConversation insert also failed:', insErr.code, insErr.message);
        // Last resort: update only (convo may already exist with different data)
        const { error: updErr } = await supa.from('conversations').update(payload).eq('id', payload.id);
        if (updErr) console.error('pushConversation all methods failed:', updErr.message);
      } else {
        console.log('pushConversation insert succeeded as fallback');
      }
    } else {
      console.log('pushConversation upsert succeeded for', payload.id);
    }
  } catch(e) { console.error('pushConversation exception:', e); }
}

async function pushMessage(convoId, msg) {
  if (!supa) return;
  try {
    const payload = { id: msg.id, convo_id: convoId, from_email: msg.from, text: msg.text, created_date: msg.created_date };
    // upsert so retries on duplicate IDs don't fail silently
    const { error } = await supa.from('messages').upsert(payload, { onConflict: 'id' });
    if (error) console.error('pushMessage error:', error.message);
  } catch(e) { console.error('pushMessage exception:', e); }
}

async function syncUsers() {
  if (!supa) return loadUsers();
  try {
    const { data, error } = await supa.from('users').select();
    if (error) throw error;
    state.users = data || [];
    localStorage.setItem('dc_users', JSON.stringify(state.users));
    return state.users;
  } catch(e) { console.warn('syncUsers failed:', e); return loadUsers(); }
}

async function syncResources() {
  if (!supa) return loadResourcesLocal();
  try {
    const { data, error } = await supa.from('resources').select();
    if (error) throw error;
    const arr = (data && data.length) ? data : loadResourcesLocal();
    state.resources = arr;
    localStorage.setItem('dc_resources', JSON.stringify(arr));
    return arr;
  } catch(e) { console.warn('syncResources failed:', e); return loadResourcesLocal(); }
}

async function syncConversations() {
  if (!state.currentUser) return [];
  if (!supa) {
    state.conversations = loadConvos().filter(c => c.participants && c.participants.includes(state.currentUser.email));
    return state.conversations;
  }
  try {
    // Try the efficient contains filter first
    let { data, error } = await supa.from('conversations').select().contains('participants', [state.currentUser.email]);
    if (error) {
      // If contains fails (wrong column type), fall back to fetching all and filtering
      console.warn('syncConversations contains() failed, falling back to full fetch:', error.message);
      const res = await supa.from('conversations').select();
      if (res.error) throw res.error;
      data = (res.data || []).filter(c => c.participants && c.participants.includes(state.currentUser.email));
    }
    const arr = data || [];
    // Merge in any local-only convos not yet in Supabase
    const local = loadConvos();
    for (const lc of local) {
      if (lc.participants && lc.participants.includes(state.currentUser.email) && !arr.find(r => r.id === lc.id)) {
        arr.push(lc);
        // Push the local-only convo up to Supabase so it persists
        pushConversation(lc);
      }
    }
    state.conversations = arr;
    saveConvos(arr);
    return arr;
  } catch(e) {
    console.warn('syncConversations failed, using local:', e);
    state.conversations = loadConvos().filter(c => c.participants && c.participants.includes(state.currentUser.email));
    return state.conversations;
  }
}

async function syncMessages(convoId) {
  if (!supa) return (loadMsgs())[convoId] || [];
  try {
    const { data, error } = await supa.from('messages').select().eq('convo_id', convoId).order('created_date', { ascending: true });
    if (error) throw error;
    const msgs = (data || []).map(r => ({ id: r.id, from: r.from_email, text: r.text, created_date: r.created_date }));
    const allMsgs = loadMsgs();
    allMsgs[convoId] = msgs;
    saveMsgs(allMsgs);
    state.messages[convoId] = msgs;
    return msgs;
  } catch(e) { console.warn('syncMessages failed:', e); return (loadMsgs())[convoId] || []; }
}

// ════ LOCAL STORAGE ════
function saveUsers(u) { localStorage.setItem('dc_users', JSON.stringify(u)); }
function loadUsers() { try { const d = localStorage.getItem('dc_users'); return d ? JSON.parse(d) : []; } catch { return []; } }
function saveConvos(c) { localStorage.setItem('dc_convos', JSON.stringify(c)); }
function loadConvos() { try { const d = localStorage.getItem('dc_convos'); return d ? JSON.parse(d) : []; } catch { return []; } }
function saveMsgs(m) { localStorage.setItem('dc_msgs', JSON.stringify(m)); }
function loadMsgs() { try { const d = localStorage.getItem('dc_msgs'); return d ? JSON.parse(d) : {}; } catch { return {}; } }
function saveNotifs(n) { localStorage.setItem('dc_notifs', JSON.stringify(n)); }
function loadNotifs() { try { const d = localStorage.getItem('dc_notifs'); return d ? JSON.parse(d) : []; } catch { return []; } }
function saveCurrentUser(u) { if (u) localStorage.setItem('dc_current', JSON.stringify(u)); else localStorage.removeItem('dc_current'); }
function loadCurrentUser() { try { const d = localStorage.getItem('dc_current'); return d ? JSON.parse(d) : null; } catch { return null; } }
function saveResourcesLocal(r) { localStorage.setItem('dc_resources', JSON.stringify(r)); }
function loadResourcesLocal() { try { const d = localStorage.getItem('dc_resources'); return d ? JSON.parse(d) : DEMO_RESOURCES; } catch { return DEMO_RESOURCES; } }

// ════ INIT ════
async function init() {
  initSupabase();
  updateKbHint();
  await syncUsers();
  await syncResources();
  const saved = loadCurrentUser();
  if (saved && saved.email_verified) {
    state.currentUser = saved;
    const fresh = state.users.find(u => u.email === saved.email);
    if (fresh) state.currentUser = fresh;
    await loadAppData();
    showPage('mentors');
    return;
  }
  showPage('landing');
}
init();

// ════ ROUTING ════
function showPage(name) {
  if (name !== 'chat') stopMessagePolling();
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  const pg = document.getElementById('page-' + name);
  if (pg) { pg.classList.add('active'); window.scrollTo(0,0); }
}

function leaveChatPage() {
  stopMessagePolling();
  showPage('messages');
}

// ════ AUTOCOMPLETE ════
let acFocusIdx = {};
function acSearch(inputId, dropdownId, list) {
  const input = document.getElementById(inputId);
  const dropdown = document.getElementById(dropdownId);
  const q = input.value.trim().toLowerCase();
  acFocusIdx[dropdownId] = -1;
  if (q.length < 1) { dropdown.classList.remove('open'); dropdown.innerHTML = ''; return; }
  const matches = list.filter(item => item.toLowerCase().includes(q)).slice(0,8);
  if (!matches.length) { dropdown.innerHTML = `<div class="autocomplete-no-results">No matches found</div>`; dropdown.classList.add('open'); return; }
  dropdown.innerHTML = matches.map((item,i) => {
    const hi = item.replace(new RegExp(`(${escReg(q)})`, 'gi'), '<mark>$1</mark>');
    return `<div class="autocomplete-item" data-idx="${i}" onmousedown="acSelect('${inputId}','${dropdownId}','${escQ(item)}')">${hi}</div>`;
  }).join('');
  dropdown.classList.add('open');
}
function acKeydown(e, dropdownId, inputId) {
  const dropdown = document.getElementById(dropdownId);
  const items = dropdown.querySelectorAll('.autocomplete-item');
  if (!dropdown.classList.contains('open') || !items.length) return;
  if (e.key === 'ArrowDown') { e.preventDefault(); acFocusIdx[dropdownId] = Math.min((acFocusIdx[dropdownId]||0)+1, items.length-1); updateAcFocus(items, acFocusIdx[dropdownId]); }
  else if (e.key === 'ArrowUp') { e.preventDefault(); acFocusIdx[dropdownId] = Math.max((acFocusIdx[dropdownId]||0)-1, 0); updateAcFocus(items, acFocusIdx[dropdownId]); }
  else if (e.key === 'Enter' && (acFocusIdx[dropdownId]||0) >= 0) { e.preventDefault(); const f = items[acFocusIdx[dropdownId]]; if(f){document.getElementById(inputId).value=f.textContent;closeAc(dropdownId);} }
  else if (e.key === 'Escape') { closeAc(dropdownId); }
}
function updateAcFocus(items, idx) { items.forEach((el,i) => el.classList.toggle('focused', i===idx)); }
function acSelect(inputId, dropdownId, value) { document.getElementById(inputId).value = value; closeAc(dropdownId); }
function closeAc(dropdownId) { const el = document.getElementById(dropdownId); if(el){el.classList.remove('open');el.innerHTML='';} }
function escReg(s) { return s.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); }
function escQ(s) { return s.replace(/'/g, "\\'"); }

// ════ PASSWORD TOGGLE ════
function togglePwd(inputId, btn) {
  const inp = document.getElementById(inputId);
  if (inp.type === 'password') { inp.type = 'text'; btn.innerHTML = `<svg viewBox="0 0 24 24"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/><line x1="1" y1="1" x2="23" y2="23"/></svg>`; }
  else { inp.type = 'password'; btn.innerHTML = `<svg viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg>`; }
}

// ════ LOGIN ════
async function handleLogin() {
  const email = document.getElementById('login-email').value.trim().toLowerCase();
  const pass  = document.getElementById('login-password').value;
  const errEl = document.getElementById('login-error');
  await syncUsers();
  let user = state.users.find(u => u.email === email && u.password === pass);
  if (!user) { const local = loadUsers(); user = local.find(u => u.email === email && u.password === pass); }
  if (!user) { errEl.textContent = 'Invalid email or password.'; errEl.style.display = 'block'; return; }
  if (!user.email_verified) { state.pendingUser = user; startVerification(user); return; }
  errEl.style.display = 'none';
  state.currentUser = user;
  saveCurrentUser(user);
  await loadAppData();
  showPage('mentors');
}

// ════ SIGNUP ════
function selectRole(role) {
  state.selectedRole = role;
  document.querySelectorAll('.role-option').forEach(el => el.classList.remove('selected'));
  document.querySelector(`.role-option[data-role="${role}"]`).classList.add('selected');
  const btn = document.getElementById('role-continue-btn');
  btn.style.opacity = '1'; btn.style.pointerEvents = 'auto';
}

function goToSignupInfo() {
  if (!state.selectedRole) return;
  document.getElementById('signup-role-label').textContent = state.selectedRole;
  document.getElementById('mentor-school-section').style.display = state.selectedRole === 'mentor' ? 'block' : 'none';
  document.getElementById('coach-teacher-school-section').style.display = (state.selectedRole==='coach'||state.selectedRole==='teacher') ? 'block' : 'none';
  document.getElementById('judge-tabroom-section').style.display = state.selectedRole === 'judge' ? 'block' : 'none';
  showPage('signup-info');
}

function toggleSchoolField() {
  document.getElementById('school-field').style.display = document.getElementById('not-in-school').checked ? 'none' : 'block';
}

async function handleSignup() {
  const first = document.getElementById('signup-first').value.trim();
  const last  = document.getElementById('signup-last').value.trim();
  const email = document.getElementById('signup-email').value.trim().toLowerCase();
  const pass  = document.getElementById('signup-password').value;
  const location = document.getElementById('signup-location').value.trim();
  const errEl = document.getElementById('signup-error');
  if (!first||!last||!email||!pass||!location) { errEl.textContent='Please fill in all required fields.'; errEl.style.display='block'; return; }
  if (pass.length < 6) { errEl.textContent='Password must be at least 6 characters.'; errEl.style.display='block'; return; }
  if (!/\S+@\S+\.\S+/.test(email)) { errEl.textContent='Please enter a valid email address.'; errEl.style.display='block'; return; }
  await syncUsers();
  if (state.users.find(u => u.email === email)) { errEl.textContent='An account with this email already exists.'; errEl.style.display='block'; return; }
  if (state.selectedRole === 'mentor') {
    const notInSchool = document.getElementById('not-in-school').checked;
    const school = document.getElementById('signup-school-mentor').value.trim();
    if (!notInSchool && !school) { errEl.textContent='Please enter your school or check "Not currently in school".'; errEl.style.display='block'; return; }
  }
  errEl.style.display = 'none';
  let school = '';
  if (state.selectedRole === 'mentor') school = document.getElementById('not-in-school').checked ? '' : document.getElementById('signup-school-mentor').value.trim();
  else if (state.selectedRole==='coach'||state.selectedRole==='teacher') school = document.getElementById('signup-school-ct').value.trim();
  const tabroom = state.selectedRole === 'judge' ? document.getElementById('signup-tabroom').value.trim() : '';
  state.pendingUser = {
    id: 'u_' + Date.now(),
    email, password: pass,
    first_name: first, last_name: last,
    role: email === 'ethav31@gmail.com' ? 'admin' : state.selectedRole,
    location, school,
    description: '',
    tabroom_username: tabroom,
    tabroom_linked: !!tabroom,
    available_for_hire: state.selectedRole !== 'coach',
    email_verified: false
  };
  startVerification(state.pendingUser);
}

// ════ VERIFICATION ════
function generateCode() { return String(Math.floor(100000 + Math.random() * 900000)); }
function startVerification(user) {
  state.verifyCode = generateCode();
  document.getElementById('verify-sub').textContent = `Sent to ${user.email}`;
  document.getElementById('verify-error').style.display = 'none';
  document.querySelectorAll('.verify-digit').forEach(d => { d.value=''; d.classList.remove('filled'); });
  showPage('verify');
  document.getElementById('verify-hint').style.display = 'flex';
  document.getElementById('verify-code-display').style.display = 'block';
  document.getElementById('verify-code-display').textContent = state.verifyCode;
  setTimeout(() => document.querySelector('.verify-digit').focus(), 100);
}

function digitInput(el, idx) {
  const val = el.value.replace(/\D/g,'');
  el.value = val ? val[0] : '';
  el.classList.toggle('filled', !!el.value);
  if (el.value) { const next = document.querySelectorAll('.verify-digit')[idx+1]; if(next) next.focus(); else checkAutoVerify(); }
}
function digitKeydown(e, el, idx) {
  if (e.key==='Backspace' && !el.value) { const prev = document.querySelectorAll('.verify-digit')[idx-1]; if(prev){prev.value='';prev.classList.remove('filled');prev.focus();} }
}
function checkAutoVerify() {
  const digits = [...document.querySelectorAll('.verify-digit')].map(d=>d.value).join('');
  if (digits.length===6) verifyCode();
}

async function verifyCode() {
  const entered = [...document.querySelectorAll('.verify-digit')].map(d=>d.value).join('');
  const errEl = document.getElementById('verify-error');
  if (entered.length < 6) { errEl.textContent='Please enter all 6 digits.'; errEl.style.display='block'; return; }
  if (entered !== state.verifyCode) {
    errEl.textContent='Incorrect code. Please try again.'; errEl.style.display='block';
    document.querySelectorAll('.verify-digit').forEach(d=>{d.value='';d.classList.remove('filled');});
    document.querySelector('.verify-digit').focus();
    return;
  }
  errEl.style.display = 'none';
  state.pendingUser.email_verified = true;
  const users = loadUsers();
  const idx = users.findIndex(u => u.email === state.pendingUser.email);
  if (idx >= 0) users[idx] = state.pendingUser; else users.push(state.pendingUser);
  saveUsers(users);
  await pushUser(state.pendingUser);
  state.users = users;
  state.currentUser = state.pendingUser;
  state.pendingUser = null;
  state.verifyCode = null;
  saveCurrentUser(state.currentUser);
  await loadAppData();
  showPage('mentors');
}

function resendCode() {
  if (!state.pendingUser) return;
  state.verifyCode = generateCode();
  document.getElementById('verify-code-display').textContent = state.verifyCode;
  document.querySelectorAll('.verify-digit').forEach(d=>{d.value='';d.classList.remove('filled');});
  document.querySelector('.verify-digit').focus();
  const errEl = document.getElementById('verify-error');
  errEl.className='alert alert-info';
  errEl.textContent='New code generated! (shown above in demo)';
  errEl.style.display='block';
  setTimeout(()=>{errEl.style.display='none';errEl.className='alert alert-error';},3000);
}

// ════ LOAD APP DATA ════
async function loadAppData() {
  state.users = loadUsers();
  await syncResources();
  await syncConversations();
  await syncNotifications();
  state.messages = loadMsgs();
  renderMentors();
  renderJudges();
  renderMessages();
  renderResources();
  renderNotifications();
  renderSettings();
  renderAllNavs();
  startNotifPolling();
}

// ════ NAV ════
function buildNav(containerId, activePage) {
  const container = document.getElementById(containerId);
  if (!container) return;
  const unreadMsgs  = state.conversations.filter(c => c.unread_by?.includes(state.currentUser?.email)).length;
  const unreadNotif = state.notifications.filter(n => !n.read).length;
  const isPhone = document.body.classList.contains('phone-view') || IS_REAL_MOBILE;
  container.innerHTML = `
    ${!isPhone ? `<div class="sidebar-logo"><div class="sidebar-logo-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="6"/></svg></div><span>Mentor Connect</span></div>` : ''}
    <div class="bottom-nav-inner">` +
    NAV_ITEMS.map(item => {
      const badge = item.id==='messages' ? unreadMsgs : item.id==='notifications' ? unreadNotif : 0;
      return `<button class="nav-item ${activePage===item.id?'active':''}" onclick="showPage('${item.page}')">${item.svg}<span>${item.label}</span>${badge>0?`<span class="nav-badge">${badge>9?'9+':badge}</span>`:''}</button>`;
    }).join('') + `</div>`;
}

function renderAllNavs() {
  buildNav('bottom-nav-mentors','mentors');
  buildNav('bottom-nav-judges','judges');
  buildNav('bottom-nav-messages','messages');
  buildNav('bottom-nav-notifs','notifications');
  buildNav('bottom-nav-resources','resources');
  buildNav('bottom-nav-settings','settings');
}

// ════ MENTORS ════
function renderMentors() {
  const mentors = state.users.filter(u => u.role==='mentor' && u.email_verified && u.email!==state.currentUser.email);
  const locs = [...new Set(mentors.map(m=>m.location).filter(Boolean))];
  const schools = [...new Set(mentors.map(m=>m.school).filter(Boolean))];
  const locSel = document.getElementById('mentor-location-filter');
  const schSel = document.getElementById('mentor-school-filter');
  locSel.innerHTML = `<option value="">All Locations</option>` + locs.map(l=>`<option value="${l}">${l}</option>`).join('');
  schSel.innerHTML = `<option value="">All Schools</option>` + schools.map(s=>`<option value="${s}">${s}</option>`).join('');
  filterMentors();
}
function filterMentors() {
  const q = document.getElementById('mentor-search').value.toLowerCase();
  const loc = document.getElementById('mentor-location-filter').value;
  const sch = document.getElementById('mentor-school-filter').value;
  const mentors = state.users.filter(u => u.role==='mentor' && u.email_verified && u.email!==state.currentUser.email);
  const filtered = mentors.filter(m => {
    const matchQ = (`${m.first_name} ${m.last_name}`).toLowerCase().includes(q) || (m.description||'').toLowerCase().includes(q);
    return matchQ && (!loc||m.location===loc) && (!sch||m.school===sch);
  });
  renderPersonList('mentors-list', filtered, 'mentor');
}

// ════ JUDGES ════
function renderJudges() { filterJudges(); }
function filterJudges() {
  const q = document.getElementById('judge-search').value.toLowerCase();
  const judges = state.users.filter(u => u.role==='judge' && u.email_verified && u.email!==state.currentUser.email);
  const filtered = judges.filter(j => (`${j.first_name} ${j.last_name}`).toLowerCase().includes(q)||(j.description||'').toLowerCase().includes(q));
  renderPersonList('judges-list', filtered, 'judge');
}

function getConvoWithUser(targetEmail) {
  return state.conversations.find(c =>
    c.participants.includes(state.currentUser.email) && c.participants.includes(targetEmail)
  );
}

function isConvoHidden(convo) {
  return (convo.hidden_by || []).includes(state.currentUser.email);
}

function renderPersonList(containerId, people, roleType) {
  const el = document.getElementById(containerId);
  if (!people.length) {
    el.innerHTML = `<div class="empty-state"><svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/></svg><p>No results found</p><small>Try a different search or filter</small></div>`;
    return;
  }

  const cards = people.map(p => {
    const avail = p.available_for_hire;
    const existingConvo = getConvoWithUser(p.email);
    const isHidden = existingConvo ? isConvoHidden(existingConvo) : false;

    const tabroomBadge = (p.role==='judge' && p.tabroom_linked)
      ? `<span class="tabroom-badge"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg>Tabroom</span>`
      : '';

    let msgBtnHtml = '';
    if (avail) {
      if (existingConvo && !isHidden) {
        // Has an active conversation - show "Open" + "Hide" button
        msgBtnHtml = `
          <div style="display:flex;gap:.35rem;align-items:center">
            <button class="msg-btn" onclick="openChat('${existingConvo.id}')">
              <svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>Open
            </button>
            <button class="msg-btn msg-hidden" onclick="hideConvo('${existingConvo.id}',event)" title="Hide conversation">
              <svg viewBox="0 0 24 24"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/><line x1="1" y1="1" x2="23" y2="23"/></svg>Hide
            </button>
          </div>`;
      } else if (existingConvo && isHidden) {
        // Conversation exists but is hidden - button reopens it
        msgBtnHtml = `
          <button class="msg-btn msg-hidden" onclick="startConvo('${p.email}')">
            <svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>Reopen Chat
          </button>`;
      } else {
        // No conversation yet
        msgBtnHtml = `
          <button class="msg-btn" onclick="startConvo('${p.email}')">
            <svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>Message
          </button>`;
      }
    }

    return `<div class="person-card">
      <div class="person-card-top">
        <div class="avatar">${(p.first_name||'?')[0]}${(p.last_name||'?')[0]}</div>
        <div class="person-card-info">
          <div class="person-name">${escHtml(p.first_name)} ${escHtml(p.last_name)}</div>
          <div class="person-role-pill">${escHtml(p.role||roleType)}</div>
          <div class="person-meta">
            ${p.location?`<span class="meta-item"><svg viewBox="0 0 24 24"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>${escHtml(p.location)}</span>`:''}
            ${p.school?`<span class="meta-item"><svg viewBox="0 0 24 24"><path d="M22 10v6M2 10l10-5 10 5-10 5z"/><path d="M6 12v5c3 3 9 3 12 0v-5"/></svg>${escHtml(p.school)}</span>`:''}
          </div>
        </div>
      </div>
      ${p.description?`<p class="person-desc">${escHtml(p.description)}</p>`:''}
      <div class="person-card-footer">
        <div style="display:flex;gap:.4rem;flex-wrap:wrap;align-items:center">
          <span class="avail-badge ${avail?'avail-yes':'avail-no'}">${avail?'Available':'Unavailable'}</span>
          ${tabroomBadge}
        </div>
        <div class="person-card-actions">${msgBtnHtml}</div>
      </div>
    </div>`;
  });

  el.innerHTML = `<div class="person-grid">${cards.join('')}</div>`;
}

// ════ HIDE / UNHIDE CONVERSATIONS ════
async function hideConvo(convoId, e) {
  if (e) e.stopPropagation();
  await syncConversations();
  const convo = state.conversations.find(c => c.id === convoId);
  if (!convo) return;
  if (!convo.hidden_by) convo.hidden_by = [];
  if (!convo.hidden_by.includes(state.currentUser.email)) {
    convo.hidden_by.push(state.currentUser.email);
  }
  saveConvos(state.conversations);
  if (supa) {
    supa.from('conversations').update({ hidden_by: convo.hidden_by }).eq('id', convoId)
      .then(({error}) => { if(error) console.warn('hide convo error:', error); });
  }
  renderMessages();
  renderMentors();
  renderJudges();
  renderAllNavs();
}

async function unhideConvo(convoId) {
  await syncConversations();
  const convo = state.conversations.find(c => c.id === convoId);
  if (!convo) return;
  if (!convo.hidden_by) convo.hidden_by = [];
  convo.hidden_by = convo.hidden_by.filter(e => e !== state.currentUser.email);
  saveConvos(state.conversations);
  if (supa) {
    supa.from('conversations').update({ hidden_by: convo.hidden_by }).eq('id', convoId)
      .then(({error}) => { if(error) console.warn('unhide convo error:', error); });
  }
}

// ════ CONVERSATIONS ════
async function startConvo(targetEmail) {
  await syncConversations();
  const existing = state.conversations.find(c =>
    c.participants.includes(state.currentUser.email) && c.participants.includes(targetEmail)
  );
  if (existing) {
    // Unhide if hidden, then open
    if (isConvoHidden(existing)) await unhideConvo(existing.id);
    renderMessages();
    renderMentors();
    renderJudges();
    openChat(existing.id);
    return;
  }
  const target = state.users.find(u => u.email === targetEmail);
  const newConvo = {
    id: 'c_' + Date.now(),
    participants: [state.currentUser.email, targetEmail],
    participant_names: {
      [state.currentUser.email]: `${state.currentUser.first_name} ${state.currentUser.last_name}`,
      [targetEmail]: target ? `${target.first_name} ${target.last_name}` : targetEmail
    },
    last_message: '',
    last_message_date: new Date().toISOString(),
    unread_by: [],
    hidden_by: []
  };
  state.conversations.push(newConvo);
  saveConvos(state.conversations);
  // Open chat immediately from local state, push to Supabase in parallel
  renderMessages();
  renderMentors();
  renderJudges();
  openChat(newConvo.id);
  // Push after opening so the local convo is already in state when openChat runs
  await pushConversation(newConvo);
}

async function openChat(convoId) {
  // Don't syncConversations here — it would overwrite a just-created local convo
  // before it's saved to Supabase. Use local state first, sync only if not found.
  let convo = state.conversations.find(c => c.id === convoId);
  if (!convo) {
    await syncConversations();
    convo = state.conversations.find(c => c.id === convoId);
  }
  if (!convo) { console.error('openChat: convo not found', convoId); return; }
  state.activeConvoId = convoId;
  if (!convo.unread_by) convo.unread_by = [];
  convo.unread_by = convo.unread_by.filter(e => e !== state.currentUser.email);
  saveConvos(state.conversations);
  if (supa) {
    supa.from('conversations').update({ unread_by: convo.unread_by }).eq('id', convoId).then(({error}) => { if(error) console.warn('unread update:', error); });
  }
  renderAllNavs();
  const otherEmail = convo.participants.find(e => e !== state.currentUser.email);
  const other = state.users.find(u => u.email === otherEmail);
  const otherName = convo.participant_names?.[otherEmail] || otherEmail;
  document.getElementById('chat-avatar').textContent = otherName.split(' ').map(n=>n[0]).join('').slice(0,2);
  document.getElementById('chat-name').textContent = otherName;
  document.getElementById('chat-role').textContent = other ? (other.role||'') : '';
  await syncMessages(convoId);
  renderChatMessages();
  showPage('chat');
  startMessagePolling(convoId);
}

// ════ MESSAGE POLLING ════
function startMessagePolling(convoId) {
  stopMessagePolling();
  state.pollInterval = setInterval(async () => {
    if (state.activeConvoId !== convoId) { stopMessagePolling(); return; }
    const before = (state.messages[convoId] || []).length;
    await syncMessages(convoId);
    const after = (state.messages[convoId] || []).length;
    if (after > before) {
      renderChatMessages();
      // If tab is hidden/backgrounded, create a notification for incoming messages
      if (document.hidden) {
        const newMsgs = (state.messages[convoId] || []).slice(before);
        for (const m of newMsgs) {
          if (m.from !== state.currentUser.email) {
            addMessageNotification(convoId, state.currentUser.email);
            break; // One notif per batch is enough
          }
        }
      }
      await syncConversations();
      renderMessages();
      renderAllNavs();
    }
  }, 3000);
}

function stopMessagePolling() {
  if (state.pollInterval) { clearInterval(state.pollInterval); state.pollInterval = null; }
}

function renderChatMessages() {
  const msgs = state.messages[state.activeConvoId] || (loadMsgs())[state.activeConvoId] || [];
  const container = document.getElementById('chat-messages-container');
  const wasAtBottom = container.scrollHeight - container.scrollTop <= container.clientHeight + 50;
  container.innerHTML = msgs.map(msg => {
    const isMine = msg.from === state.currentUser.email;
    const time = new Date(msg.created_date).toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'});
    return `<div class="message ${isMine?'mine':'other'}"><div><div class="message-bubble">${escHtml(msg.text)}</div><div class="message-time">${time}</div></div></div>`;
  }).join('');
  if (wasAtBottom || msgs.length <= 1) container.scrollTop = container.scrollHeight;
}

function chatKeydown(e) { if (e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendMessage();} }

async function sendMessage() {
  const input = document.getElementById('chat-input');
  const text = input.value.trim();
  if (!text || !state.activeConvoId) return;
  const convoId = state.activeConvoId;
  const msg = { id:'m_'+Date.now(), from:state.currentUser.email, text, created_date:new Date().toISOString() };

  // 1. Optimistically add to local state + storage immediately
  if (!state.messages[convoId]) state.messages[convoId] = [];
  state.messages[convoId].push(msg);
  const allMsgs = loadMsgs();
  allMsgs[convoId] = state.messages[convoId];
  saveMsgs(allMsgs);
  input.value = '';
  renderChatMessages();

  // 2. Find convo in current state BEFORE any sync (so we don't lose it)
  let convo = state.conversations.find(c => c.id === convoId);

  // 3. If not found locally, try syncing to find it
  if (!convo) {
    await syncConversations();
    convo = state.conversations.find(c => c.id === convoId);
  }

  // 4. Push message to Supabase
  await pushMessage(convoId, msg);

  // 5. Update convo metadata and push
  if (convo) {
    convo.last_message = text;
    convo.last_message_date = msg.created_date;
    if (!convo.unread_by) convo.unread_by = [];
    const others = convo.participants.filter(e => e !== state.currentUser.email);
    for (const ep of others) {
      if (!convo.unread_by.includes(ep)) convo.unread_by.push(ep);
      addMessageNotification(convoId, ep);
    }
    if (convo.hidden_by) convo.hidden_by = convo.hidden_by.filter(e => e !== state.currentUser.email);
    saveConvos(state.conversations);
    await pushConversation(convo);
  } else {
    console.warn('sendMessage: could not find convo', convoId, '- message was still sent');
  }

  renderMessages();
}

function renderMessages() {
  const el = document.getElementById('conversations-list');
  // Only show non-hidden conversations
  const convos = state.conversations.filter(c => !isConvoHidden(c));
  if (!convos.length) {
    el.innerHTML = `<div class="empty-state"><svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg><p>No conversations yet</p><small>Start a conversation by messaging a mentor or judge</small></div>`;
    return;
  }
  el.innerHTML = [...convos].sort((a,b)=>new Date(b.last_message_date)-new Date(a.last_message_date)).map(c => {
    const otherEmail = c.participants.find(e=>e!==state.currentUser.email);
    const otherName = c.participant_names?.[otherEmail] || otherEmail;
    const initials = otherName.split(' ').map(n=>n[0]).join('').slice(0,2);
    const unread = c.unread_by?.includes(state.currentUser.email);
    const dateStr = c.last_message_date ? new Date(c.last_message_date).toLocaleDateString([],{month:'short',day:'numeric'}) : '';
    return `<div class="convo-card ${unread?'unread':''}" onclick="openChat('${c.id}')">
      <div class="convo-inner">
        <div class="avatar avatar-sm">${escHtml(initials)}</div>
        <div class="convo-meta">
          <div class="convo-row1"><span class="convo-name">${escHtml(otherName)}</span><span class="convo-date">${dateStr}</span></div>
          <div class="convo-row2"><span class="convo-preview">${escHtml(c.last_message||'No messages yet')}</span>
            <div class="convo-end">
              ${unread?'<span class="unread-dot"></span>':''}
              <button class="hide-btn" onclick="hideConvo('${c.id}',event)" title="Hide conversation">
                <svg viewBox="0 0 24 24"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/><line x1="1" y1="1" x2="23" y2="23"/></svg>
              </button>
              <span class="chev-icon"><svg viewBox="0 0 24 24"><path d="M9 18 L15 12 L9 6"/></svg></span>
            </div>
          </div>
        </div>
      </div>
    </div>`;
  }).join('');
}

// ════ RESOURCES ════
function filterResources() {
  const cat = document.getElementById('resource-category-filter').value;
  const filtered = cat ? state.resources.filter(r=>r.category===cat) : state.resources;
  const el = document.getElementById('resources-list');
  if (!filtered.length) { el.innerHTML=`<div class="empty-state"><svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg><p>No resources in this category</p></div>`; return; }
  el.innerHTML = `<div class="resources-grid">` + filtered.map(r => {
    const icon = TYPE_ICONS[r.type] || TYPE_ICONS.default;
    const catBadge = CAT_BADGE[r.category] || 'badge-gray';
    const dateStr = new Date(r.created_date).toLocaleDateString([],{month:'short',day:'numeric',year:'numeric'});
    return `<div class="resource-card" onclick="openResourceDetail('${r.id}')">
      <div class="resource-inner">
        <div class="resource-icon">${icon}</div>
        <div class="resource-body">
          <div class="resource-title-row"><h3 class="resource-title">${escHtml(r.title)}</h3>${r.url&&r.url!=='#'?`<a href="${r.url}" target="_blank" class="resource-link" onclick="event.stopPropagation()"><svg viewBox="0 0 24 24"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><path d="M15 3 L21 3 L21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg></a>`:''}</div>
          <div class="resource-footer"><span class="badge ${catBadge}">${(r.category||'').replace('_',' ')}</span><span class="badge badge-outline">${r.type}</span></div>
          ${r.description?`<p class="resource-desc">${escHtml(r.description)}</p>`:''}
          <div class="resource-footer" style="margin-top:.45rem">${r.posted_by_name?`<span>By ${escHtml(r.posted_by_name)}</span>`:''}<span>${dateStr}</span></div>
        </div>
      </div>
    </div>`;
  }).join('') + `</div>`;
}

function renderResources() {
  const btn = document.getElementById('admin-create-resource-btn');
  btn.style.display = (state.currentUser && state.currentUser.role==='admin') ? 'inline-flex' : 'none';
  filterResources();
}

function openResourceDetail(resourceId) {
  const resource = state.resources.find(r=>r.id===resourceId);
  if (!resource) return;
  state.currentResourceId = resourceId;
  renderResourceDetail();
  showPage('resource-detail');
}

function renderResourceDetail() {
  const r = state.resources.find(x=>x.id===state.currentResourceId);
  if (!r) return;
  const dateStr = new Date(r.created_date).toLocaleDateString([],{month:'long',day:'numeric',year:'numeric'});
  const icon = TYPE_ICONS[r.type] || TYPE_ICONS.default;
  const catBadge = CAT_BADGE[r.category] || 'badge-gray';
  const isAdmin = state.currentUser && state.currentUser.role==='admin';
  document.getElementById('resource-detail-content').innerHTML = `
    <div style="max-width:800px">
      <div style="display:flex;gap:1.5rem;margin-bottom:2rem">
        <div style="width:60px;height:60px;background:var(--blue-100);border-radius:var(--radius);display:flex;align-items:center;justify-content:center;flex-shrink:0">${icon}</div>
        <div style="flex:1"><h1 style="margin-bottom:.5rem;color:var(--blue-900)">${escHtml(r.title)}</h1>
          <div style="display:flex;gap:.5rem;flex-wrap:wrap;margin-bottom:.8rem"><span class="badge ${catBadge}">${(r.category||'').replace('_',' ')}</span><span class="badge badge-outline">${r.type}</span></div>
          <small style="color:#666">${r.posted_by_name?`Posted by ${escHtml(r.posted_by_name)} • `:''}Updated ${dateStr}</small>
        </div>
      </div>
      ${r.url&&r.url!=='#'?`<div style="background:var(--blue-50);border-radius:var(--radius);padding:1rem;margin-bottom:2rem;border-left:4px solid var(--blue-600)"><p style="margin-bottom:.5rem;color:#666;font-size:.9rem">Resource Link</p><a href="${r.url}" target="_blank" style="color:var(--blue-600);word-break:break-all">${escHtml(r.url)}</a></div>`:''}
      ${r.description?`<div style="margin-bottom:2rem"><p style="color:var(--blue-900);line-height:1.6;white-space:pre-wrap">${escHtml(r.description)}</p></div>`:''}
      ${isAdmin?`<div style="border-top:1px solid var(--blue-200);padding-top:1.5rem;margin-top:2rem">
        <h3 style="margin-bottom:1rem;color:var(--blue-900)">Admin: Edit Resource</h3>
        <div class="form-group"><label class="form-label">Title</label><input id="edit-res-title" type="text" class="form-input" value="${escHtml(r.title)}" /></div>
        <div class="form-group"><label class="form-label">Description / Content</label><textarea id="edit-res-desc" class="form-textarea">${escHtml(r.description||'')}</textarea></div>
        <div class="form-group"><label class="form-label">Link / URL</label><input id="edit-res-url" type="text" class="form-input" value="${escHtml(r.url||'')}" placeholder="https://" /></div>
        <div class="grid-2">
          <div class="form-group"><label class="form-label">Category</label><select id="edit-res-cat" class="form-select"><option value="debate" ${r.category==='debate'?'selected':''}>Debate</option><option value="public_speaking" ${r.category==='public_speaking'?'selected':''}>Public Speaking</option><option value="coaching" ${r.category==='coaching'?'selected':''}>Coaching</option><option value="judging" ${r.category==='judging'?'selected':''}>Judging</option><option value="general" ${r.category==='general'?'selected':''}>General</option></select></div>
          <div class="form-group"><label class="form-label">Type</label><select id="edit-res-type" class="form-select"><option value="video" ${r.type==='video'?'selected':''}>Video</option><option value="document" ${r.type==='document'?'selected':''}>Document</option><option value="link" ${r.type==='link'?'selected':''}>Link</option><option value="default" ${r.type==='default'?'selected':''}>Other</option></select></div>
        </div>
        <div style="display:flex;gap:.6rem;margin-top:1.2rem"><button class="btn btn-primary" onclick="saveResourceEdit()">Save Changes</button><button class="btn btn-danger-outline" onclick="deleteResource('${r.id}')">Delete Resource</button></div>
      </div>`:''}
    </div>`;
  buildNav('bottom-nav-resource-detail','resources');
}

async function saveResourceEdit() {
  const r = state.resources.find(x=>x.id===state.currentResourceId);
  if (!r) return;
  r.title = document.getElementById('edit-res-title').value.trim();
  r.description = document.getElementById('edit-res-desc').value.trim();
  r.url = document.getElementById('edit-res-url').value.trim();
  r.category = document.getElementById('edit-res-cat').value;
  r.type = document.getElementById('edit-res-type').value;
  r.updated_date = new Date().toISOString();
  saveResourcesLocal(state.resources);
  await pushResource(r);
  renderResourceDetail();
  alert('Resource updated! Changes are live for all users.');
}

async function deleteResource(resourceId) {
  if (!confirm('Delete this resource? This cannot be undone.')) return;
  const idx = state.resources.findIndex(r=>r.id===resourceId);
  if (idx >= 0) state.resources.splice(idx, 1);
  saveResourcesLocal(state.resources);
  await deleteResourceFromSupabase(resourceId);
  showPage('resources');
  renderResources();
}

function showCreateResourceModal() { document.getElementById('create-resource-modal').classList.add('open'); document.getElementById('new-res-title').focus(); }
function closeCreateResourceModal() {
  document.getElementById('create-resource-modal').classList.remove('open');
  ['new-res-title','new-res-desc','new-res-url'].forEach(id => document.getElementById(id).value='');
  document.getElementById('new-res-cat').value=''; document.getElementById('new-res-type').value='';
}

async function createNewResource() {
  const title = document.getElementById('new-res-title').value.trim();
  const desc = document.getElementById('new-res-desc').value.trim();
  const url = document.getElementById('new-res-url').value.trim();
  const cat = document.getElementById('new-res-cat').value;
  const type = document.getElementById('new-res-type').value;
  if (!title||!cat||!type) { alert('Please fill in Title, Category, and Type'); return; }
  const isAdmin = state.currentUser.role === 'admin';
  const newResource = {
    id: 'r' + Date.now(), title, description: desc, url: url||'#', category: cat, type,
    posted_by_name: isAdmin ? 'The Mentor Connect Team' : state.currentUser.first_name + ' ' + state.currentUser.last_name,
    created_date: new Date().toISOString(), updated_date: new Date().toISOString()
  };
  state.resources.push(newResource);
  saveResourcesLocal(state.resources);
  await pushResource(newResource);
  addResourceNotification(newResource);
  closeCreateResourceModal();
  renderResources();
  alert('Resource created and is now live for all users!');
}

// ════ NOTIFICATION HELPERS ════
// ════ SUPABASE NOTIFICATION SYNC ════
async function pushNotification(notif) {
  if (!supa) return false;
  try {
    const payload = {
      id: notif.id,
      user_email: notif.user_email,
      type: notif.type,
      title: notif.title,
      message: notif.message,
      reference_id: notif.reference_id || null,
      from_user_name: notif.from_user_name || null,
      read: false,
      created_date: notif.created_date
    };
    const { error } = await supa.from('notifications').upsert(payload, { onConflict: 'id' });
    if (error) {
      console.error('pushNotification error:', error.message, error.code, error.details);
      return false;
    }
    return true;
  } catch(e) { console.error('pushNotification exception:', e); return false; }
}

async function syncNotifications() {
  if (!state.currentUser) return;
  const local = loadNotifs().filter(n => n.user_email === state.currentUser.email);
  if (!supa) { state.notifications = local; return; }
  try {
    const { data, error } = await supa.from('notifications')
      .select()
      .eq('user_email', state.currentUser.email)
      .order('created_date', { ascending: false });
    if (error) throw error;
    const remote = data || [];
    // Merge: remote takes priority, but keep any local-only ones not yet in remote
    const remoteIds = new Set(remote.map(n => n.id));
    const localOnly = local.filter(n => !remoteIds.has(n.id));
    // Push local-only notifs to Supabase
    for (const n of localOnly) { pushNotification(n); }
    state.notifications = [...remote, ...localOnly].sort((a,b) => new Date(b.created_date) - new Date(a.created_date));
    // Update local cache
    const otherLocal = loadNotifs().filter(n => n.user_email !== state.currentUser.email);
    saveNotifs([...otherLocal, ...state.notifications]);
  } catch(e) {
    console.warn('syncNotifications failed, using local:', e);
    state.notifications = local;
  }
}

async function markNotifReadInSupabase(notifId) {
  if (!supa) return;
  try {
    await supa.from('notifications').update({ read: true }).eq('id', notifId);
  } catch(e) { console.warn('markNotifRead error:', e); }
}

async function markAllNotifsReadInSupabase() {
  if (!supa || !state.currentUser) return;
  try {
    await supa.from('notifications').update({ read: true })
      .eq('user_email', state.currentUser.email).eq('read', false);
  } catch(e) { console.warn('markAllNotifsRead error:', e); }
}

// ════ NOTIFICATION HELPERS ════
async function addNotification(targetEmail, type, title, message, referenceId, fromUserName) {
  const notif = {
    id: 'n_' + Date.now() + '_' + Math.random().toString(36).slice(2,6),
    user_email: targetEmail,
    type,
    title,
    message,
    reference_id: referenceId || null,
    from_user_name: fromUserName || null,
    read: false,
    created_date: new Date().toISOString()
  };
  // Always save locally first so it works even if Supabase fails
  const allNotifs = loadNotifs();
  allNotifs.push(notif);
  saveNotifs(allNotifs);
  // Push to Supabase
  const saved = await pushNotification(notif);
  if (!saved) console.warn('Notification not saved to Supabase for', targetEmail, '- stored locally only');
  // If this is for the current user, update live UI immediately
  if (state.currentUser && state.currentUser.email === targetEmail) {
    if (!state.notifications) state.notifications = [];
    state.notifications.unshift(notif);
    renderNotifications();
    renderAllNavs();
  }
  return notif;
}

async function addMessageNotification(convoId, toEmail) {
  const senderName = `${state.currentUser.first_name} ${state.currentUser.last_name}`;
  // Check Supabase for existing unread notif for this convo to avoid spam
  if (supa) {
    try {
      const { data } = await supa.from('notifications')
        .select('id').eq('user_email', toEmail).eq('type', 'message')
        .eq('reference_id', convoId).eq('read', false).limit(1);
      if (data && data.length > 0) return; // already has unread notif
    } catch(e) { /* fallback to local check */ }
  } else {
    const allNotifs = loadNotifs();
    const existing = allNotifs.find(n =>
      n.user_email === toEmail && n.type === 'message' &&
      n.reference_id === convoId && !n.read
    );
    if (existing) return;
  }
  await addNotification(
    toEmail, 'message',
    `New message from ${senderName}`,
    `${senderName} sent you a message`,
    convoId, senderName
  );
}

async function addResourceNotification(resource) {
  const isAdmin = state.currentUser.role === 'admin';
  const postedBy = isAdmin ? 'The Mentor Connect Team' : `${state.currentUser.first_name} ${state.currentUser.last_name}`;
  const allUsers = loadUsers();
  for (const u of allUsers) {
    if (u.email === state.currentUser.email) continue;
    if (!u.email_verified) continue;
    await addNotification(
      u.email, 'new_resource',
      `New resource: ${resource.title}`,
      `${postedBy} posted a new ${resource.category.replace('_',' ')} resource`,
      resource.id, postedBy
    );
  }
}

// Background poll: check for new notifications every 8s while logged in
let notifPollInterval = null;
function startNotifPolling() {
  if (notifPollInterval) return;
  notifPollInterval = setInterval(async () => {
    if (!state.currentUser) return;
    const prevCount = state.notifications.filter(n=>!n.read).length;
    await syncNotifications();
    const newCount = state.notifications.filter(n=>!n.read).length;
    if (newCount !== prevCount) {
      renderNotifications();
      renderAllNavs();
    }
  }, 8000);
}
function stopNotifPolling() {
  if (notifPollInterval) { clearInterval(notifPollInterval); notifPollInterval = null; }
}

// ════ NOTIFICATIONS ════
const NOTIF_ICONS = {
  message:      `<svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>`,
  hire_request: `<svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/></svg>`,
  new_resource: `<svg viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>`,
  default:      `<svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>`
};

function renderNotifications() {
  const unread = state.notifications.filter(n=>!n.read).length;
  const badge = document.getElementById('notif-unread-badge');
  const markBtn = document.getElementById('mark-all-btn');
  badge.textContent=unread; badge.style.display=unread>0?'inline-flex':'none';
  markBtn.style.display=unread>0?'inline-flex':'none';
  const el = document.getElementById('notifications-list');
  if (!state.notifications.length) { el.innerHTML=`<div class="empty-state"><svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg><p>No notifications yet</p><small>You'll see messages and new resources here</small></div>`; return; }
  el.innerHTML = [...state.notifications].sort((a,b)=>new Date(b.created_date)-new Date(a.created_date)).map(n => {
    const icon = NOTIF_ICONS[n.type]||NOTIF_ICONS.default;
    const dateStr = new Date(n.created_date).toLocaleDateString([],{month:'short',day:'numeric'});
    return `<div class="notif-card ${!n.read?'unread':''}" onclick="handleNotifClick('${n.id}')">
      <div class="notif-inner"><div class="notif-icon">${icon}</div><div class="notif-body">
        <div class="notif-row1">
          <span class="notif-title">${escHtml(n.title)}</span>
          <div style="display:flex;align-items:center;gap:.25rem;flex-shrink:0">
            <span class="notif-date">${dateStr}</span>
            <div class="notif-actions">
              ${!n.read ? `<button class="notif-action-btn" onclick="dismissNotification('${n.id}',event)" title="Mark as read">
                <svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg>
              </button>` : ''}
              <button class="notif-action-btn delete" onclick="deleteNotification('${n.id}',event)" title="Delete">
                <svg viewBox="0 0 24 24"><polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M9 6V4h6v2"/></svg>
              </button>
            </div>
          </div>
        </div>
        <p class="notif-message">${escHtml(n.message)}</p>
        ${n.from_user_name?`<p class="notif-from">From: ${escHtml(n.from_user_name)}</p>`:''}
      </div></div>
    </div>`;
  }).join('');
}

async function handleNotifClick(id) {
  const n = state.notifications.find(x=>x.id===id);
  if (!n) return;
  if (!n.read) {
    n.read = true;
    await markNotifReadInSupabase(id);
    // update local cache
    const allLocal = loadNotifs();
    const idx = allLocal.findIndex(x=>x.id===id);
    if (idx>=0) { allLocal[idx].read=true; saveNotifs(allLocal); }
  }
  renderNotifications(); renderAllNavs();
  if ((n.type==='message'||n.type==='hire_request') && n.reference_id) openChat(n.reference_id);
  else if (n.type==='new_resource') showPage('resources');
}

async function markAllRead() {
  state.notifications.forEach(n => n.read = true);
  await markAllNotifsReadInSupabase();
  const allLocal = loadNotifs();
  allLocal.filter(n=>n.user_email===state.currentUser.email).forEach(n=>n.read=true);
  saveNotifs(allLocal);
  renderNotifications(); renderAllNavs();
}

async function deleteNotification(id, e) {
  e.stopPropagation();
  state.notifications = state.notifications.filter(n => n.id !== id);
  const allLocal = loadNotifs().filter(n => n.id !== id);
  saveNotifs(allLocal);
  if (supa) {
    const { error } = await supa.from('notifications').delete().eq('id', id);
    if (error) console.error('deleteNotification supabase error:', error.message, error.code);
    else console.log('deleteNotification: removed', id, 'from Supabase');
  }
  renderNotifications(); renderAllNavs();
}

async function dismissNotification(id, e) {
  e.stopPropagation();
  const n = state.notifications.find(x => x.id === id);
  if (!n) return;
  n.read = true;
  await markNotifReadInSupabase(id);
  const allLocal = loadNotifs();
  const idx = allLocal.findIndex(x => x.id === id);
  if (idx >= 0) { allLocal[idx].read = true; saveNotifs(allLocal); }
  renderNotifications(); renderAllNavs();
}

// ════ SETTINGS TABS + SWIPE ════
let settingsTabIdx = 0;

function switchSettingsTab(idx) {
  settingsTabIdx = idx;
  const track = document.getElementById('settings-track');
  if (track) track.style.transform = `translateX(-${idx * 100}%)`;
  document.querySelectorAll('.settings-tab').forEach((t, i) => t.classList.toggle('active', i === idx));
}

function initSettingsSwipe() {
  const track = document.getElementById('settings-track');
  if (!track) return;
  let startX = 0, startY = 0, isDragging = false, didSwipe = false;
  const numPanels = () => document.querySelectorAll('.settings-panel:not([style*="display:none"])').length;

  track.addEventListener('touchstart', e => {
    startX = e.touches[0].clientX;
    startY = e.touches[0].clientY;
    isDragging = true; didSwipe = false;
    track.style.transition = 'none';
  }, { passive: true });

  track.addEventListener('touchmove', e => {
    if (!isDragging) return;
    const dx = e.touches[0].clientX - startX;
    const dy = e.touches[0].clientY - startY;
    if (!didSwipe && Math.abs(dy) > Math.abs(dx)) { isDragging = false; return; }
    didSwipe = true;
    const base = settingsTabIdx * 100;
    const pct = (dx / track.offsetWidth) * 100;
    track.style.transform = `translateX(calc(-${base}% + ${dx}px))`;
  }, { passive: true });

  track.addEventListener('touchend', e => {
    if (!isDragging) return;
    isDragging = false;
    track.style.transition = '';
    const dx = e.changedTouches[0].clientX - startX;
    const maxIdx = numPanels() - 1;
    if (Math.abs(dx) > 55) {
      const newIdx = dx < 0
        ? Math.min(settingsTabIdx + 1, maxIdx)
        : Math.max(settingsTabIdx - 1, 0);
      switchSettingsTab(newIdx);
    } else {
      switchSettingsTab(settingsTabIdx); // snap back
    }
  }, { passive: true });
}

// ════ SETTINGS ════
function renderSettings() {
  const u = state.currentUser;
  document.getElementById('settings-avatar').textContent = `${(u.first_name||'?')[0]}${(u.last_name||'?')[0]}`;
  document.getElementById('settings-name').textContent = `${u.first_name} ${u.last_name}`;
  document.getElementById('settings-email').textContent = u.email;
  document.getElementById('settings-email-sub').textContent = u.email;
  document.getElementById('settings-role-tag').textContent = u.role||'';
  document.getElementById('settings-first').value = u.first_name||'';
  document.getElementById('settings-last').value = u.last_name||'';
  document.getElementById('settings-location').value = u.location||'';
  document.getElementById('settings-bio').value = u.description||'';
  const schoolWrap = document.getElementById('settings-school-wrap');
  if (u.role==='mentor'||u.role==='coach'||u.role==='teacher') { schoolWrap.style.display='block'; document.getElementById('settings-school').value=u.school||''; }
  else { schoolWrap.style.display='none'; }
  const availRow = document.getElementById('settings-avail-row');
  const availChk = document.getElementById('settings-available');
  if (u.role==='coach') { availRow.style.display='none'; }
  else { availRow.style.display='flex'; availChk.checked = u.available_for_hire!==false; }
  const tabroomSec = document.getElementById('settings-tabroom-section');
  if (u.role==='judge') { tabroomSec.style.display='block'; document.getElementById('settings-tabroom').value=u.tabroom_username||''; renderTabroomStatus(); }
  else { tabroomSec.style.display='none'; }
  // Show admin tab if admin
  const adminTab = document.getElementById('stab-2');
  const adminPanel = document.getElementById('spanel-2');
  if (u.role === 'admin') {
    if (adminTab) adminTab.style.display = '';
    if (adminPanel) adminPanel.style.display = '';
  } else {
    if (adminTab) adminTab.style.display = 'none';
    if (adminPanel) adminPanel.style.display = 'none';
  }
  // Reset to first tab on open
  switchSettingsTab(0);
  // Init swipe (safe to call multiple times)
  initSettingsSwipe();
  syncUsers().then(renderAdminPanel);
}

function renderTabroomStatus() {
  const u = state.currentUser;
  const el = document.getElementById('settings-tabroom-status');
  if (u.tabroom_linked) {
    el.innerHTML = `<div class="tabroom-status linked"><svg viewBox="0 0 24 24"><path d="M20 6 L9 17 L4 12"/></svg>Tabroom account linked: <strong>${escHtml(u.tabroom_username)}</strong></div>`;
  } else {
    el.innerHTML = `<div class="tabroom-status unlinked"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>No Tabroom account linked yet</div>`;
  }
}

function linkTabroom() {
  const username = document.getElementById('settings-tabroom').value.trim();
  if (!username) { const errEl=document.getElementById('settings-error'); errEl.textContent='Please enter a Tabroom username first.'; errEl.style.display='block'; setTimeout(()=>errEl.style.display='none',3000); return; }
  state.currentUser.tabroom_username = username;
  state.currentUser.tabroom_linked = true;
  persistCurrentUserChanges();
  renderTabroomStatus();
  const s=document.getElementById('settings-success'); s.querySelector('span').textContent=`Tabroom account "${username}" linked successfully!`; s.style.display='flex'; setTimeout(()=>s.style.display='none',3500);
}

async function saveSettings() {
  const u = state.currentUser;
  u.first_name = document.getElementById('settings-first').value.trim();
  u.last_name = document.getElementById('settings-last').value.trim();
  u.location = document.getElementById('settings-location').value.trim();
  u.description = document.getElementById('settings-bio').value.trim();
  if (u.role!=='coach') u.available_for_hire = document.getElementById('settings-available').checked;
  else u.available_for_hire = false;
  if (u.role==='mentor'||u.role==='coach'||u.role==='teacher') u.school = document.getElementById('settings-school').value.trim();
  if (u.role==='judge') u.tabroom_username = document.getElementById('settings-tabroom').value.trim();
  await persistCurrentUserChanges();
  renderSettings(); renderMentors(); renderJudges();
  const s=document.getElementById('settings-success'); s.querySelector('span').textContent='Profile updated successfully!'; s.style.display='flex'; setTimeout(()=>s.style.display='none',3000);
}

async function persistCurrentUserChanges() {
  const users = loadUsers();
  const idx = users.findIndex(u=>u.email===state.currentUser.email);
  if (idx>=0) users[idx]=state.currentUser; else users.push(state.currentUser);
  saveUsers(users);
  saveCurrentUser(state.currentUser);
  state.users = users;
  await pushUser(state.currentUser);
}

function handleLogout() {
  stopMessagePolling();
  stopNotifPolling();
  saveCurrentUser(null);
  state.currentUser = null;
  state.conversations = [];
  state.notifications = [];
  state.messages = {};
  showPage('landing');
}

// ════ ACCOUNT DELETION ════
async function promptDeleteAccount() {
  if (!confirm('Are you sure you want to delete your account? This cannot be undone.')) return;
  if (!confirm('This will permanently delete all your data. Click OK to confirm.')) return;
  const userId = state.currentUser.id;
  const userEmail = state.currentUser.email;
  await deleteUserFromSupabase(userId);
  const users = loadUsers();
  const idx = users.findIndex(u=>u.email===userEmail);
  if (idx>=0) { users.splice(idx,1); saveUsers(users); }
  const convos = loadConvos();
  saveConvos(convos.filter(c=>!c.participants.includes(userEmail)));
  saveCurrentUser(null);
  stopMessagePolling();
  stopNotifPolling();
  state.currentUser = null;
  alert('Your account has been deleted.');
  showPage('landing');
}

async function deleteUserAccount(email) {
  if (!state.currentUser || (state.currentUser.role!=='admin' && state.currentUser.email!==email)) { alert('Only admins can delete other users.'); return; }
  if (!confirm(`Delete account for ${email}? This cannot be undone.`)) return;
  const targetUser = state.users.find(u=>u.email===email);
  const userId = targetUser?.id;
  if (userId) await deleteUserFromSupabase(userId);
  const users = loadUsers();
  const idx = users.findIndex(u=>u.email===email);
  if (idx>=0) { users.splice(idx,1); saveUsers(users); state.users = users; }
  if (state.currentUser.email === email) {
    saveCurrentUser(null); state.currentUser = null; showPage('landing');
  } else { renderAdminPanel(); renderMentors(); renderJudges(); }
}

// ════ ADMIN ════
async function filterAdminUsers() { await syncUsers(); renderAdminPanel(); }

function renderAdminPanel() {
  const adminPanel = document.getElementById('admin-panel');
  if (!state.currentUser || state.currentUser.role!=='admin') { adminPanel.style.display='none'; return; }
  adminPanel.style.display = 'block';
  const usersList = document.getElementById('admin-users-list');
  const searchVal = document.getElementById('admin-search-users').value.toLowerCase();
  let filtered = state.users;
  if (searchVal) filtered = state.users.filter(u=>(`${u.first_name} ${u.last_name}`).toLowerCase().includes(searchVal)||u.email.toLowerCase().includes(searchVal));
  if (!filtered.length) { usersList.innerHTML = searchVal?'<p style="color:#999">No users found</p>':'<p style="color:#999">No users in the system</p>'; return; }
  usersList.innerHTML = filtered.map(u => `
    <div style="display:flex;justify-content:space-between;align-items:center;padding:.8rem;border:1px solid #e5e7eb;border-radius:var(--radius);margin-bottom:.6rem">
      <div style="flex:1">
        <p style="font-weight:500;font-size:.9rem">${escHtml(u.first_name)} ${escHtml(u.last_name)}</p>
        <small style="color:#666">${escHtml(u.email)}</small>
        <div style="margin-top:.4rem;">
          <select class="form-select" onchange="changeUserRole('${u.email}', this.value)" style="font-size:.85rem;padding:.4rem">
            <option value="mentor" ${u.role==='mentor'?'selected':''}>Mentor</option>
            <option value="coach" ${u.role==='coach'?'selected':''}>Coach</option>
            <option value="judge" ${u.role==='judge'?'selected':''}>Judge</option>
            <option value="teacher" ${u.role==='teacher'?'selected':''}>Teacher</option>
            <option value="admin" ${u.role==='admin'?'selected':''}>Admin</option>
          </select>
        </div>
      </div>
      <button class="btn btn-sm btn-danger-outline" onclick="deleteUserAccount('${u.email.replace(/'/g,"\\'")}')">Delete</button>
    </div>`).join('');
}

async function changeUserRole(email, newRole) {
  const user = state.users.find(u=>u.email===email);
  if (!user) return;
  user.role = newRole;
  saveUsers(state.users);
  await pushUser(user);
  if (state.currentUser && state.currentUser.email===email) { state.currentUser.role=newRole; saveCurrentUser(state.currentUser); }
  renderAdminPanel();
  alert(`User role changed to ${newRole}`);
}

// ════ UTILS ════
function escHtml(str) {
  if (!str) return '';
  return String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;').replace(/'/g,'&#39;');
}
</script>
</body>
</html>
