<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, viewport-fit=cover" />
<title>JUNCTION — Train Search &amp; Booking</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;600;700&family=IBM+Plex+Mono:wght@500;600&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --royal-orange:#FF6A13;
    --royal-orange-dim:#C24E0C;
    --imperial-blue:#0B3D91;
    --imperial-blue-light:#3E6FD1;
    --black:#0C0C10;
    --black-soft:#1B1410;
    --yellow:#FFC72C;
    --white:#FFFDF9;
    --grey:#C9A98F;
    font-size:16px;
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(circle at 15% 8%, rgba(255,106,19,.25) 0%, transparent 45%),
      radial-gradient(circle at 85% 92%, rgba(11,61,145,.35) 0%, transparent 45%),
      var(--black);
    min-height:100vh;
    display:flex;
    align-items:flex-start;
    justify-content:center;
    font-family:'Inter',Arial,sans-serif;
    color:var(--white);
    padding:24px 12px;
  }
  .display{font-family:'Rajdhani',Arial,sans-serif;}
  .mono{font-family:'IBM Plex Mono',monospace;}

  /* ---------- Phone frame ---------- */
  .phone{
    width:100%;
    max-width:410px;
    background:var(--black-soft);
    border-radius:34px;
    border:6px solid #050506;
    box-shadow:0 30px 60px -20px rgba(0,0,0,.7), 0 0 0 1px rgba(255,106,19,.25);
    overflow:hidden;
    position:relative;
    min-height:820px;
    display:flex;
    flex-direction:column;
  }

  /* ---------- Header / signature split-flap ---------- */
  .header{
    position:relative;
    padding:14px 20px 20px;
    background:
      repeating-linear-gradient(115deg, transparent 0 26px, rgba(255,255,255,.05) 26px 27px),
      linear-gradient(150deg,var(--royal-orange) 0%, var(--royal-orange-dim) 40%, var(--imperial-blue) 100%);
    border-bottom:3px solid var(--yellow);
    overflow:hidden;
  }
  .header::after{
    content:"";position:absolute;right:-40px;top:-40px;width:150px;height:150px;
    background:var(--yellow);opacity:.22;border-radius:50%;filter:blur(10px);
  }
  .brand-row{display:flex;align-items:center;justify-content:space-between;position:relative;z-index:1;}
  .flapboard{display:flex;gap:4px;}
  .flap{
    width:26px;height:34px;background:#050506;border-radius:4px;
    display:flex;align-items:center;justify-content:center;
    font-family:'Rajdhani',Arial,sans-serif;font-weight:700;font-size:20px;color:var(--yellow);
    border:1px solid #2a2a33;
    box-shadow:inset 0 -2px 0 rgba(255,255,255,.08);
  }
  .tagline{margin:8px 2px 0;font-size:12.5px;color:#1a0f08;font-weight:600;letter-spacing:.02em;position:relative;z-index:1;}
  .badge-live{
    display:inline-flex;align-items:center;gap:5px;background:rgba(12,12,16,.35);
    border:1px solid rgba(12,12,16,.5);color:var(--black);
    font-size:10.5px;font-weight:700;padding:4px 9px;border-radius:20px;letter-spacing:.05em;
  }
  .badge-live::before{content:"";width:6px;height:6px;border-radius:50%;background:var(--black);animation:pulse 1.4s infinite;}
  @keyframes pulse{0%,100%{opacity:1}50%{opacity:.25}}

  /* ---------- Tabs ---------- */
  .tabs{display:flex;gap:8px;padding:16px 20px 0;}
  .tab-btn{
    flex:1;padding:11px 6px;border-radius:12px;border:1.5px solid #3a2a1e;
    background:var(--black-soft);color:var(--grey);font-family:'Rajdhani',Arial,sans-serif;
    font-weight:600;font-size:14.5px;letter-spacing:.03em;cursor:pointer;transition:.2s;
  }
  .tab-btn.active{
    background:linear-gradient(135deg,var(--royal-orange),var(--royal-orange-dim));
    color:var(--white);border-color:var(--royal-orange);
    box-shadow:0 6px 16px -6px rgba(255,106,19,.7);
  }

  .content{flex:1;overflow-y:auto;padding:16px 20px 90px;}
  .content::-webkit-scrollbar{width:0}

  /* ---------- Cards / inputs ---------- */
  .card{
    background:#221812;border:1px solid #3a2a1e;border-radius:16px;
    padding:16px;margin-bottom:14px;
  }
  .field{position:relative;margin-bottom:10px;}
  .field label{display:block;font-size:11px;text-transform:uppercase;letter-spacing:.08em;color:var(--yellow);margin-bottom:5px;font-weight:600;}
  .field input{
    width:100%;background:#150e0a;border:1.5px solid #3a2a1e;border-radius:10px;
    padding:12px 12px 12px 38px;color:var(--white);font-size:15px;font-family:'Inter',Arial,sans-serif;
    outline:none;transition:.15s;
  }
  .field input:focus{border-color:var(--royal-orange);box-shadow:0 0 0 3px rgba(255,106,19,.2);}
  .field .icon{position:absolute;left:12px;top:37px;color:var(--royal-orange);pointer-events:none;}
  .swap-row{display:flex;align-items:center;gap:8px;}
  .swap-col{flex:1;}
  .swap-btn{
    width:38px;height:38px;border-radius:50%;border:1.5px solid var(--royal-orange);
    background:rgba(255,106,19,.15);color:var(--royal-orange);display:flex;align-items:center;
    justify-content:center;cursor:pointer;flex-shrink:0;margin-top:14px;transition:.2s;
  }
  .swap-btn:active{transform:rotate(180deg);}

  .suggest-list{
    position:absolute;left:0;right:0;top:100%;margin-top:4px;background:#1a120d;
    border:1px solid #3a2a1e;border-radius:10px;overflow:hidden;z-index:20;
    max-height:220px;overflow-y:auto;box-shadow:0 12px 24px -8px rgba(0,0,0,.6);
  }
  .suggest-item{padding:10px 12px;display:flex;justify-content:space-between;align-items:center;cursor:pointer;border-bottom:1px solid #2a1e16;}
  .suggest-item:last-child{border-bottom:none;}
  .suggest-item:hover{background:rgba(255,199,44,.1);}
  .suggest-code{font-family:'IBM Plex Mono',monospace;color:var(--yellow);font-weight:600;font-size:12.5px;background:rgba(255,199,44,.12);padding:2px 6px;border-radius:5px;}
  .suggest-name{font-size:13.5px;color:var(--white);}
  .suggest-sub{font-size:11px;color:var(--grey);}

  .date-field input[type=date]{color-scheme:dark;}

  .btn-primary{
    width:100%;padding:13px;border:none;border-radius:12px;cursor:pointer;
    background:linear-gradient(135deg,var(--yellow),var(--royal-orange));
    color:var(--black);font-family:'Rajdhani',Arial,sans-serif;font-weight:700;font-size:16px;
    letter-spacing:.03em;box-shadow:0 8px 18px -8px rgba(255,199,44,.6);transition:.15s;
  }
  .btn-primary:active{transform:scale(.98);}
  .btn-ghost{
    width:100%;padding:11px;border-radius:12px;background:transparent;
    border:1.5px solid var(--imperial-blue-light);color:#a9c0f5;font-weight:600;
    font-family:'Rajdhani',Arial,sans-serif;font-size:14.5px;cursor:pointer;
  }

  .section-title{font-family:'Rajdhani',Arial,sans-serif;font-weight:700;font-size:15px;letter-spacing:.04em;
    color:var(--white);margin:4px 0 10px;display:flex;align-items:center;gap:8px;}
  .section-title .dot{width:8px;height:8px;border-radius:2px;background:var(--royal-orange);}

  /* nearby station ticket-stub card */
  .stub{
    display:flex;background:#221812;border:1px solid #3a2a1e;border-radius:14px;
    margin-bottom:10px;overflow:hidden;position:relative;
  }
  .stub-main{flex:1;padding:12px 14px;}
  .stub-dist{
    width:64px;flex-shrink:0;background:linear-gradient(180deg,var(--royal-orange),var(--royal-orange-dim));
    display:flex;flex-direction:column;align-items:center;justify-content:center;
    border-left:2px dashed var(--yellow);position:relative;
  }
  .stub-dist b{font-family:'Rajdhani',Arial,sans-serif;font-size:19px;color:var(--black);}
  .stub-dist span{font-size:9px;color:#3a1c08;letter-spacing:.05em;font-weight:600;}
  .stub-top{display:flex;align-items:center;gap:8px;margin-bottom:3px;}
  .stub-code{font-family:'IBM Plex Mono',monospace;font-weight:600;color:var(--yellow);font-size:13px;background:rgba(255,199,44,.14);padding:2px 7px;border-radius:5px;}
  .stub-name{font-weight:600;font-size:14.5px;}
  .stub-city{font-size:12px;color:var(--grey);margin-top:2px;}
  .stub-actions{display:flex;gap:6px;margin-top:8px;}
  .chip-btn{
    font-size:11px;padding:5px 9px;border-radius:8px;border:1px solid #3a2a1e;
    background:#150e0a;color:#e8d8c8;cursor:pointer;font-weight:600;
  }
  .chip-btn:active{background:rgba(255,199,44,.18);border-color:var(--yellow);}

  .empty{text-align:center;padding:40px 12px;color:var(--grey);}
  .empty svg{opacity:.6;margin-bottom:10px;}
  .empty p{font-size:13.5px;line-height:1.5;margin:4px 0 0;}

  /* train result card (boarding-pass) */
  .train-card{background:#221812;border:1px solid #3a2a1e;border-radius:16px;margin-bottom:14px;overflow:hidden;}
  .train-top{padding:14px 16px 10px;display:flex;justify-content:space-between;align-items:flex-start;}
  .train-name{font-family:'Rajdhani',Arial,sans-serif;font-weight:700;font-size:16px;}
  .train-no{font-size:11px;color:var(--grey);font-family:'IBM Plex Mono',monospace;}
  .train-tag{font-size:10px;background:rgba(255,106,19,.22);color:var(--royal-orange);border:1px solid var(--royal-orange);padding:3px 8px;border-radius:20px;font-weight:700;letter-spacing:.03em;}
  .journey-row{display:flex;align-items:center;padding:6px 16px 14px;gap:10px;}
  .journey-time{font-family:'Rajdhani',Arial,sans-serif;font-size:22px;font-weight:700;}
  .journey-code{font-size:11px;color:var(--grey);}
  .journey-mid{flex:1;text-align:center;position:relative;}
  .journey-mid .line{height:2px;background:repeating-linear-gradient(90deg,var(--yellow) 0 6px,transparent 6px 11px);margin:0 4px;}
  .journey-mid .dur{font-size:10.5px;color:var(--grey);margin-top:4px;}
  .perforation{border-top:2px dashed #3a2a1e;position:relative;margin:0 16px;}
  .perforation::before,.perforation::after{
    content:"";position:absolute;top:-9px;width:18px;height:18px;background:var(--black-soft);border-radius:50%;
  }
  .perforation::before{left:-26px;}
  .perforation::after{right:-26px;}
  .classes{display:flex;gap:8px;padding:14px 16px;flex-wrap:wrap;}
  .class-chip{
    flex:1;min-width:84px;border:1.5px solid #3a2a1e;border-radius:10px;padding:8px 10px;cursor:pointer;
    text-align:center;transition:.15s;background:#150e0a;
  }
  .class-chip.sel{border-color:var(--royal-orange);background:rgba(255,106,19,.15);}
  .class-chip b{display:block;font-family:'Rajdhani',Arial,sans-serif;font-size:14px;}
  .class-chip .price{color:var(--yellow);font-weight:700;font-size:13.5px;margin-top:2px;}
  .class-chip .seats{font-size:9.5px;color:var(--grey);margin-top:1px;}
  .book-row{padding:0 16px 16px;}

  /* modal */
  .overlay{position:absolute;inset:0;background:rgba(4,4,6,.72);display:flex;align-items:flex-end;z-index:50;backdrop-filter:blur(2px);}
  .sheet{
    width:100%;background:#1B1410;border-radius:22px 22px 0 0;padding:20px 20px 26px;
    border-top:3px solid var(--royal-orange);max-height:88%;overflow-y:auto;
  }
  .sheet-handle{width:38px;height:4px;background:#4a3626;border-radius:4px;margin:0 auto 16px;}
  .sheet h3{font-family:'Rajdhani',Arial,sans-serif;font-size:19px;margin:0 0 14px;}
  .pax-row{display:flex;gap:8px;margin-bottom:10px;}
  .pax-row input, .pax-row select{
    background:#150e0a;border:1.5px solid #3a2a1e;border-radius:9px;padding:10px;color:var(--white);font-size:13.5px;
  }
  .pax-row input{flex:2;} .pax-row .age{flex:1;width:0;} .pax-row select{flex:1;}
  .close-x{position:absolute;right:18px;top:16px;background:none;border:none;color:var(--grey);font-size:20px;cursor:pointer;}

  /* ticket confirmation */
  .ticket{
    background:linear-gradient(160deg,var(--royal-orange) 0%, var(--imperial-blue) 100%);
    border-radius:16px;padding:18px;border:1px solid var(--yellow);position:relative;overflow:hidden;
  }
  .ticket::after{content:"";position:absolute;inset:0;background:repeating-linear-gradient(115deg,transparent 0 30px, rgba(255,255,255,.06) 30px 31px);}
  .pnr-box{background:rgba(0,0,0,.35);border:1px dashed var(--yellow);border-radius:10px;padding:10px;text-align:center;margin-top:12px;position:relative;z-index:1;}
  .pnr-box b{font-family:'IBM Plex Mono',monospace;font-size:20px;letter-spacing:.08em;color:var(--yellow);}
  .barcode{display:flex;gap:2px;height:34px;margin-top:12px;align-items:flex-end;position:relative;z-index:1;}
  .barcode span{background:var(--yellow);width:3px;display:inline-block;}

  /* ---------- Bottom nav ---------- */
  .bottomnav{
    position:absolute;bottom:0;left:0;right:0;background:#150e0a;border-top:2px solid var(--royal-orange-dim);
    display:flex;padding:8px 10px calc(10px + env(safe-area-inset-bottom));
  }
  .nav-btn{
    flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;background:none;border:none;
    color:var(--grey);cursor:pointer;padding:6px 0;font-family:'Rajdhani',Arial,sans-serif;font-size:11px;font-weight:600;letter-spacing:.03em;
  }
  .nav-btn.active{color:var(--royal-orange);}
  .nav-btn.active svg{filter:drop-shadow(0 0 6px rgba(255,106,19,.7));}

  .toast{
    position:absolute;left:20px;right:20px;bottom:84px;background:var(--yellow);color:var(--black);
    padding:11px 14px;border-radius:10px;font-size:13px;font-weight:600;text-align:center;z-index:60;
    box-shadow:0 10px 24px -8px rgba(0,0,0,.5);
  }

  .booking-item{display:flex;justify-content:space-between;align-items:center;padding:12px 14px;}
  .booking-item + .booking-item{border-top:1px solid #3a2a1e;}
  .cancel-btn{background:none;border:1px solid #6a2a2a;color:#f0a0a0;border-radius:8px;padding:6px 10px;font-size:11px;cursor:pointer;}

  /* ---------- Offline status strip ---------- */
  .conn-strip{
    margin:12px 20px 0;padding:8px 12px;border-radius:10px;font-size:11.5px;font-weight:600;
    display:flex;align-items:center;gap:7px;letter-spacing:.02em;
    background:rgba(62,111,209,.14);border:1px solid var(--imperial-blue-light);color:#a9c0f5;
    transition:.2s;
  }
  .conn-strip.offline{
    background:rgba(255,106,19,.14);border-color:var(--royal-orange);color:#ffb27a;
  }
  .conn-strip .conn-dot{width:7px;height:7px;border-radius:50%;background:#4a90ff;flex-shrink:0;}
  .conn-strip.offline .conn-dot{background:var(--royal-orange);animation:pulse 1.4s infinite;}
</style>
</head>
<body>

<div class="phone" id="phone">
  <div class="header">
    <div class="brand-row">
      <div class="flapboard" id="flapboard"></div>
      <span class="badge-live" id="connBadge">LIVE</span>
    </div>
    <div class="tagline">Find your nearest station. Book your next journey.</div>
  </div>

  <div class="tabs">
    <button class="tab-btn active" data-tab="search" type="button">🚄&nbsp; Book Trains</button>
    <button class="tab-btn" data-tab="nearby" type="button">🧭&nbsp; Nearby Stations</button>
  </div>

  <div class="conn-strip" id="connStrip"></div>

  <div class="content" id="content"></div>

  <div class="bottomnav">
    <button class="nav-btn active" data-tab="search" type="button">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="3" width="16" height="13" rx="3"/><path d="M4 11h16M8 21l1.5-3h5L16 21M9 16v1M15 16v1"/></svg>
      Book
    </button>
    <button class="nav-btn" data-tab="nearby" type="button">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="9"/><path d="M15 9l-3.5 1.5L10 14l3.5-1.5L15 9z"/></svg>
      Nearby
    </button>
    <button class="nav-btn" data-tab="bookings" type="button">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 8a2 2 0 012-2h14a2 2 0 012 2v2a2 2 0 000 4v2a2 2 0 01-2 2H5a2 2 0 01-2-2v-2a2 2 0 000-4V8z"/><path d="M13 6v12" stroke-dasharray="2 2"/></svg>
      Bookings
    </button>
    <button class="nav-btn" data-tab="profile" type="button">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="3.5"/><path d="M4.5 20c1.5-4 5-5.5 7.5-5.5s6 1.5 7.5 5.5"/></svg>
      Profile
    </button>
  </div>
</div>

<script>
"use strict";

/* =======================================================
   JUNCTION — Train search & nearby-station finder
   Written in a typed style (JSDoc types below stand in for
   TypeScript's interfaces) and compiled to plain JS so the
   app runs with zero external script dependency — no build
   step, no CDN transpiler, works offline once the page loads.
   ======================================================= */

/**
 * @typedef {{code:string,name:string,city:string,state:string,lat:number,lon:number}} Station
 * @typedef {{name:string,age:string,gender:string}} Passenger
 * @typedef {{pnr:string,trainName:string,trainNo:string,from:string,to:string,date:string,cls:string,price:number,passengers:Passenger[]}} Booking
 * @typedef {{code:string,label:string,price:number,seats:number}} TrainClass
 * @typedef {{no:string,name:string,depTime:string,arrTime:string,durMins:number,classes:TrainClass[]}} TrainResult
 */

/** @type {Station[]} */
/* Compact dataset: [code, name, state, lat, lon] — derived from the Datameet Indian Railways
   open dataset (CC-BY), filtered to entries with valid coordinates & state. Expanded below. */
const STATIONS_RAW = [["AA","Ataria","Uttar Pradesh",27.1725,80.861],["AABH","Ambika Bhawani Halt","Bihar",25.7514,84.969],["AADR","Amb Andora","Punjab",31.6707,76.1104],["AAG","Angar","Maharashtra",17.9543,75.5999],["AAH","Itehar","Madhya Pradesh",26.5626,78.6838],["AAK","Ankai Kila","Maharashtra",20.1832,74.4369],["AAL","Amlai","Madhya Pradesh",23.1735,81.5935],["AAM","Angadippuram","Kerala",10.9795,76.2073],["AAP","Ambiapur","Uttar Pradesh",26.5154,79.8269],["AAR","Adesar","Gujarat",23.553,70.9721],["AAS","Asranada","Rajasthan",26.3974,73.3053],["AAY","Aralvaymozhi","Tamil Nadu",8.2495,77.5293],["AB","Ambur","Tamil Nadu",12.7829,78.7213],["ABB","Abada","West Bengal",22.5479,88.1994],["ABD","Ambli Road","Gujarat",23.0544,72.494],["ABE","Ambikeshwar","Madhya Pradesh",26.3686,77.9638],["ABEO","Pattaravakkam","Tamil Nadu",13.1145,80.1662],["ABFC","Ambari Falakata","West Bengal",26.6416,88.5106],["ABGT","Arbagatta Halt","Karnataka",13.9075,76.135],["ABH","Ambarnath","Maharashtra",19.2104,73.1848],["ABI","Ambaturai","Tamil Nadu",10.2722,77.9245],["ABKA","Ambika Kalna","West Bengal",23.2114,88.354],["ABKP","Ambikapur","Chhattisgarh",23.137,83.1451],["ABLE","Ambale","Maharashtra",18.3987,74.1715],["ABO","Asthal Bohar","Haryana",28.8578,76.6292],["ABP","Akbarpur","Uttar Pradesh",26.4298,82.5391],["ABR","Abu Road","Rajasthan",24.4708,72.7757],["ABS","Abohar","Punjab",30.1394,74.1957],["ABSA","Ambassa","Tripura",23.9305,91.8608],["ABU","Ambattur","Tamil Nadu",13.1143,80.1525],["ABW","Abutara Halt","West Bengal",26.0708,89.5869],["ABX","Ambari","Maharashtra",19.6894,78.2167],["ABY","Ambivli","Maharashtra",19.2676,73.1717],["ABZ","Adgaon Buzurg","Maharashtra",21.1239,76.9559],["ACAB","A-Cabin Bondamunda","Orissa",22.2397,84.9456],["ACG","Achegaon","Maharashtra",20.9762,75.9342],["ACH","Achalganj","Uttar Pradesh",26.4446,80.5454],["ACK","Acharapakkam","Tamil Nadu",12.4068,79.8217],["ACL","Ancheli","Gujarat",20.8452,72.945],["ACLE","Azimganj City","West Bengal",24.2379,88.2589],["ACN","Adhichchanur","Tamil Nadu",12.0541,79.1825],["ACND","Acharya Narendra Dev Nagar","Uttar Pradesh",26.7746,82.157],["ACU","Achuara Halt","Bihar",25.4527,85.6611],["AD","Adoni","Andhra Pradesh",15.617,77.2749],["ADB","Adilabad","Andhra Pradesh",19.6801,78.5353],["ADD","Adas Road","Gujarat",22.4866,73.0347],["ADE","Adari Road","Gujarat",20.9868,70.3308],["ADF","Adina","Jharkhand",25.1144,88.1232],["ADH","Andheri","Maharashtra",19.1174,72.8469],["ADHL","Adihalli","Karnataka",13.2834,76.3473],["ADI","Ahmedabad Jn","Gujarat",23.0255,72.6015],["ADIJ","Ahmedabad Jn","Gujarat",23.0257,72.6017],["ADL","Andul","West Bengal",22.5752,88.2396],["ADQ","Adhikari","West Bengal",26.5769,88.1699],["ADR","Mandi Adampur","Haryana",29.2831,75.4689],["ADRA","Adra Jn","West Bengal",23.496,86.6747],["ADRE",# Rail-gadi
Developed by Mr op panda for railway 
