<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Find Your Top Local Agent</title>
  <style>
    :root{
      --brand:#0061ff; --brand-2:#6aa5ff; --accent:#ff6b6b; --accent-2:#ffd166;
      --ink:#0f172a; --muted:#64748b; --line:rgba(15,23,42,.12); --ok:#16a34a;
    }
    html,body{margin:0;height:100%;background:
      radial-gradient(1200px 600px at 10% -10%,rgba(255,107,107,.24),transparent 60%),
      radial-gradient(1200px 600px at 110% 10%,rgba(106,165,255,.24),transparent 60%),
      linear-gradient(180deg,#f7f9ff,#eff3ff 30%,#f9fbff 100%);
      color:var(--ink); font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif}
    *,*:before,*:after{box-sizing:border-box}

    .wrap{max-width:820px;margin:40px auto;padding:0 20px}
    .card{
      background:rgba(255,255,255,.9);
      border:1px solid rgba(255,255,255,.6);
      border-radius:22px; padding:28px 26px 24px;
      box-shadow:0 10px 25px -15px rgba(0,0,0,.25), 0 80px 140px -120px rgba(0,97,255,.45);
      backdrop-filter:saturate(1.2) blur(6px);
      transition:padding .2s ease, background .2s ease, border-color .2s ease, box-shadow .2s ease;
    }

    /* HERO MODE (step 1 only a search bar) */
    .hero .card{ background:transparent; border-color:transparent; box-shadow:none; padding:0; }
    .hero [data-hide-on-hero]{display:none !important;}

    /* Progress line only (no numbers) */
    .progress{height:10px;background:#eef2f7;border-radius:999px;overflow:hidden;margin:0 0 22px}
    .bar{height:100%;width:0;background:linear-gradient(90deg,var(--brand),var(--brand-2),var(--accent),var(--accent-2));
      background-size:200% 100%;animation:slide 10s linear infinite;transition:width .25s}
    @keyframes slide{from{background-position:0% 0}to{background-position:200% 0}}

    .step{display:none;animation:fade .18s ease}
    .step.active{display:block}
    @keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}

    /* Inputs */
    label{display:block;margin:6px 0 8px;font-weight:800}
    .visually-hidden{position:absolute!important;height:1px;width:1px;overflow:hidden;clip:rect(1px,1px,1px,1px);white-space:nowrap}
    input{width:100%;padding:14px 14px 14px 44px;border:1px solid var(--line);border-radius:14px;font-size:15px;background:#fff;color:var(--ink)}
    input:focus{outline:none;border-color:#c9d7ff;box-shadow:0 0 0 5px #eaf0ff}
    .field{position:relative}
    .ficon{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:#94a3b8}
    .ficon svg{width:18px;height:18px;display:block}

    /* Search pill (hero) */
    .searchwrap{position:relative;margin-top:6px}
    .searchbar{
      position:relative;z-index:2;width:100%;
      padding:18px 64px 18px 54px; /* space for icon + kbd */
      border-radius:999px;border:1px solid #dce3f0;background:#fff;font-size:17px;line-height:1.35;
      box-shadow:0 25px 50px -22px rgba(0,0,0,.25);
    }
    .icon,.kbd{position:absolute;top:50%;transform:translateY(-50%);pointer-events:none}
    .icon{left:16px;color:#64748b}
    .icon svg{width:18px;height:18px;display:block}
    .kbd{right:14px;font-size:12px;color:#93a0b6;border:1px solid #dbe1ee;border-radius:6px;padding:3px 6px;background:#f7f9ff}

    .tiny{font-size:12px;color:#7c879a;margin-top:10px}
    .err{color:#b00020;margin-top:6px;display:none}

    /* Choice chips */
    .choices{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px;margin-top:10px}
    .choice{display:flex;align-items:center;justify-content:center;gap:8px;padding:16px 12px;border:1px solid var(--line);
      border-radius:14px;background:#fff;font-weight:800;cursor:pointer;user-select:none;
      transition:transform .05s,box-shadow .12s,border-color .12s,background .12s}
    .choice:hover{border-color:#cfd6e3;box-shadow:0 14px 22px -18px rgba(0,0,0,.28)}
    .choice:active{transform:translateY(1px)}
    .choice.selected{outline:2px solid var(--brand);background:#f5f8ff}
    @media(max-width:520px){.choices{grid-template-columns:1fr}}

    .btns{display:flex;gap:12px;margin-top:16px}
    button{cursor:pointer;border:0;padding:12px 16px;border-radius:12px;font-weight:800;letter-spacing:.2px}
    .next{background:var(--brand);color:#fff;box-shadow:0 10px 22px -10px rgba(0,97,255,.6)}
    .back{background:#eef2f7;color:#2b3c55}
    .submit{background:var(--ok);color:#fff;box-shadow:0 10px 22px -12px rgba(22,163,74,.5)}

    #status{min-height:16px;margin-top:8px;color:var(--muted)}
    .pac-container,.pac-item,.pac-item span,.pac-matched{font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif!important;font-size:14px!important}

    @media (prefers-reduced-motion: reduce){ .bar{animation:none} .step{animation:none} }
  </style>
</head>
<body>
  <div class="wrap hero" id="shell">
    <div class="card">
      <!-- PROGRESS (only the sliding line; hidden in hero/step1) -->
      <div id="controls" data-hide-on-hero>
        <div class="progress"><div class="bar" id="bar"></div></div>
      </div>

      <form id="form" novalidate>
        <!-- 1) ADDRESS (hero search only) -->
        <div class="step active" data-step="1">
          <label class="visually-hidden" for="address">Search your property address</label>
          <div class="searchwrap">
            <span class="icon" aria-hidden="true">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                   stroke-linecap="round" stroke-linejoin="round">
                <circle cx="11" cy="11" r="7"></circle>
                <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
              </svg>
            </span>
            <input id="address" name="address" class="searchbar" type="text"
                   placeholder="Search your property address…" autocomplete="off"
                   autocapitalize="off" autocorrect="off" spellcheck="false" required />
            <span class="kbd">Enter</span>
          </div>
          <div class="tiny">Powered by Google</div>

          <!-- Hidden address fields -->
          <input type="hidden" id="addressFull" name="addressFull" />
          <input type="hidden" id="placeId" name="placeId" />
          <input type="hidden" id="streetNumber" name="streetNumber" />
          <input type="hidden" id="route" name="route" />
          <input type="hidden" id="locality" name="locality" />
          <input type="hidden" id="state" name="state" />
          <input type="hidden" id="addrPostcode" name="addressPostcode" />
          <input type="hidden" id="lat" name="lat" />
          <input type="hidden" id="lng" name="lng" />

          <div id="addrErr" class="err">Please select an address from the list, or use manual entry.</div>
          <div class="tiny" style="margin-top:8px"><a href="#" id="manualToggle">Can’t find it? Enter address manually.</a></div>

          <div id="manual" style="display:none;margin-top:10px">
            <div style="display:grid;gap:10px;grid-template-columns:1fr">
              <input id="m_street" placeholder="Street address (e.g. 10 Example St)" />
              <input id="m_suburb" placeholder="Suburb" />
              <div style="display:grid;gap:10px;grid-template-columns:2fr 1fr">
                <input id="m_state" placeholder="State (e.g. NSW)" />
                <input id="m_postcode" inputmode="numeric" maxlength="4" placeholder="Postcode" />
              </div>
            </div>
            <div class="tiny">Manual entry accepted if autocomplete isn’t available.</div>
          </div>

          <div class="btns" data-hide-on-hero>
            <button type="button" id="addrNext" class="next" style="display:none">Next</button>
          </div>
        </div>

        <!-- 2) PROPERTY TYPE (nicer layout) -->
        <div class="step" data-step="2">
          <label class="visually-hidden">Property type</label>
          <input type="hidden" id="ptype" name="propertyType" required />
          <div class="choices" id="ptypeChoices">
            <div class="choice" data-value="House">🏠 House</div>
            <div class="choice" data-value="Unit">🏢 Unit</div>
            <div class="choice" data-value="Townhouse">🏘️ Townhouse</div>
            <div class="choice" data-value="Land">🧱 Land</div>
            <div class="choice" data-value="Other">🧩 Other</div>
          </div>
          <div class="btns"><button type="button" class="back">Back</button></div>
        </div>

        <!-- 3) PRICE -->
        <div class="step" data-step="3">
          <label class="visually-hidden">Estimated price point</label>
          <input type="hidden" id="price" name="priceBand" required />
          <div class="choices" id="priceChoices">
            <div class="choice" data-value="0–500k">0–500k</div>
            <div class="choice" data-value="500k–1m">500k–1m</div>
            <div class="choice" data-value="$1m–$2m">$1m–$2m</div>
            <div class="choice" data-value="$2m+">$2m+</div>
          </div>
          <div class="btns"><button type="button" class="back">Back</button></div>
        </div>

        <!-- 4) PHONE (with icon) -->
        <div class="step" data-step="4">
          <label class="visually-hidden" for="phone">Best phone number</label>
          <div class="field">
            <span class="ficon" aria-hidden="true">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                   stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.86 19.86 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6A19.86 19.86 0 0 1 2.08 4.18 2 2 0 0 1 4.06 2h3a2 2 0 0 1 2 1.72c.12.9.37 1.77.73 2.58a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.5-1.25a2 2 0 0 1 2.11-.45c.81.36 1.68.61 2.58.73A2 2 0 0 1 22 16.92z"></path>
              </svg>
            </span>
            <input id="phone" name="phone" inputmode="tel" maxlength="20" placeholder="Best phone number" required />
          </div>
          <div class="tiny">We’ll only use this to share your shortlist and next steps.</div>
          <div class="btns"><button type="button" class="back">Back</button><button type="button" class="next">Next</button></div>
        </div>

        <!-- 5) EMAIL (with icon) -->
        <div class="step" data-step="5">
          <label class="visually-hidden" for="email">Email for your results</label>
          <div class="field">
            <span class="ficon" aria-hidden="true">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                   stroke-linecap="round" stroke-linejoin="round">
                <path d="M4 4h16v16H4z" opacity=".1"></path>
                <path d="M4 4h16v16H4z"></path>
                <path d="m22 6-10 7L2 6"></path>
              </svg>
            </span>
            <input id="email" name="email" type="email" inputmode="email" autocomplete="email" placeholder="Email for your results" required />
          </div>
          <div class="tiny">By continuing, you agree we may contact you about your appraisal. You can opt out anytime.</div>
          <div class="btns"><button type="button" class="back">Back</button><button type="submit" class="submit">See my match</button></div>
        </div>

        <!-- 6) THANK YOU -->
        <div class="step" data-step="6">
          <div style="text-align:center;padding:10px 2px">
            <h2 style="margin:6px 0 10px">Thank you for completing! 🎉</h2>
            <p style="margin:0 0 10px;color:var(--muted)">We will send you the match very shortly!</p>
            <p class="tiny">Keep an eye on your email and phone for the details.</p>
          </div>
        </div>
      </form>

      <div id="status" class="tiny" aria-live="polite" data-hide-on-hero></div>
    </div>
  </div>

  <script>
  (function(){
    const WEBHOOK_URL = "https://script.google.com/macros/s/AKfycbwQW0E9h0gWIN-wX6CDKUQJrjXzkfgA6sTIpqwd3-HisuiiJGXgcgSsUl5HWH8uV48/exec";
    const PROJECT = "agent-matcher-v1";
    const formSteps = 5;

    const $ = s => document.querySelector(s);
    const form = $("#form");
    const steps = [...form.querySelectorAll(".step")];
    const bar = $("#bar");
    const controls = $("#controls");
    const statusEl = $("#status");
    const shell = $("#shell");

    let stepIndex = 0, manualMode = false, addressSelected = false;

    function setHeroMode(on){ shell.classList.toggle("hero", !!on); }
    function showStep(i){
      stepIndex = Math.max(0, Math.min(i, steps.length-1));
      steps.forEach((s,idx)=>s.classList.toggle("active", idx===stepIndex));
      const pct = Math.round((Math.min(stepIndex+1, formSteps)/formSteps)*100);
      if (bar) bar.style.width = pct+"%";
      if (controls) controls.style.display = stepIndex===0 ? "none" : "";
      if (statusEl) statusEl.style.display = stepIndex===0 ? "none" : "";
      setHeroMode(stepIndex===0);
    }
    function fieldsForStep(){
      const step = steps[stepIndex];
      const inputs = step ? step.querySelectorAll("input, select") : [];
      const data = {}; inputs.forEach(inp => data[inp.name||inp.id] = (inp.value||"").trim()); return data;
    }
    function valid(){
      const step = steps[stepIndex]; if(!step) return true;
      if (stepIndex===0){
        if (manualMode){
          const street=$("#m_street").value.trim(), suburb=$("#m_suburb").value.trim(),
                st=$("#m_state").value.trim(), pc=$("#m_postcode").value.trim();
          if(!street||!suburb||!st||pc.length!==4) return false;
          $("#address").value = `${street}, ${suburb} ${st} ${pc}`;
          $("#addressFull").value = $("#address").value;
          $("#placeId").value=""; $("#streetNumber").value=""; $("#route").value="";
          $("#locality").value=suburb; $("#state").value=st; $("#addrPostcode").value=pc; $("#lat").value=""; $("#lng").value="";
          $("#addrErr").style.display="none"; return true;
        } else {
          if(!addressSelected || !$("#placeId").value){ $("#addrErr").style.display="block"; return false; }
        }
      }
      const req = step.querySelectorAll("input[required],select[required]");
      for(const inp of req){
        const v=(inp.value||"").trim(); if(!v) return false;
        if(inp.type==="email"){ if(inp.checkValidity && !inp.checkValidity()) return false; if(!inp.checkValidity && !/.+@.+\..+/.test(v)) return false; }
      }
      return true;
    }
    function save(eventType){
      const key = PROJECT+":leadId"; let leadId = localStorage.getItem(key);
      if(!leadId){ leadId = (crypto.randomUUID?crypto.randomUUID():Date.now()+Math.random().toString(16).slice(2)); localStorage.setItem(key,leadId); }
      const payload = {leadId,event:eventType,stepNumber:stepIndex+1,answers:fieldsForStep(),ts:new Date().toISOString(),ua:navigator.userAgent,screen:{w:innerWidth,h:innerHeight,dpr:devicePixelRatio||1}};
      const body = JSON.stringify(payload);
      let ok=false; try{ fetch(WEBHOOK_URL,{method:"POST",mode:"no-cors",body,keepalive:true}); ok=true; }catch(_){ try{ if(navigator.sendBeacon){ navigator.sendBeacon(WEBHOOK_URL,new Blob([body],{type:"text/plain"})); ok=true; } }catch(__){} }
      if (statusEl && stepIndex>0) statusEl.textContent = ok ? "Saved." : "Working offline—will retry on next step.";
    }
    function next(){ if(!valid()){ if(statusEl && stepIndex>0){ statusEl.textContent="Please complete this step."; } return; } save("step"); showStep(stepIndex+1); }

    form.addEventListener("click",(e)=>{
      const el = e.target.closest && e.target.closest(".choice,.next,.back,#addrNext");
      if(!el) return;
      if(el.classList.contains("back")){ showStep(stepIndex-1); return; }
      if(el.id==="addrNext" || el.classList.contains("next")){ next(); return; }
      if(el.classList.contains("choice")){
        const group = el.parentElement; group.querySelectorAll(".choice").forEach(c=>c.classList.remove("selected"));
        el.classList.add("selected");
        if(group.id==="ptypeChoices") $("#ptype").value = el.dataset.value;
        if(group.id==="priceChoices") $("#price").value = el.dataset.value;
        save("step"); showStep(stepIndex+1);
      }
    });
    form.addEventListener("submit",(e)=>{ e.preventDefault(); if(!valid()){ if(statusEl){ statusEl.textContent="Please complete this step."; } return; } save("complete"); showStep(formSteps); });

    // Manual toggle for address
    $("#manualToggle").addEventListener("click",(e)=>{ e.preventDefault(); manualMode=!manualMode; $("#manual").style.display = manualMode ? "block" : "none"; $("#addrNext").style.display = manualMode ? "inline-block" : "none"; addressSelected=false; $("#placeId").value=""; $("#addrErr").style.display="none"; });
    // Address events
    $("#address").addEventListener("keydown",(e)=>{ if(e.key==="Enter"){ if(manualMode){ e.preventDefault(); next(); } else { e.preventDefault(); } } });
    $("#address").addEventListener("input",()=>{ addressSelected=false; $("#placeId").value=""; $("#addrErr").style.display="none"; });

    // Google Places init (callback)
    window.initPlaces = function(){
      const input = document.getElementById("address");
      if(!window.google || !google.maps || !google.maps.places || !input){ document.getElementById("addrNext").style.display="inline-block"; return; }
      const ac = new google.maps.places.Autocomplete(input,{
        types:["address"], componentRestrictions:{country:"au"},
        fields:["address_components","formatted_address","geometry","place_id"]
      });
      ac.addListener("place_changed", ()=>{
        const p = ac.getPlace(); if(!p || !p.address_components) return;
        addressSelected = true; $("#addrErr").style.display="none";
        $("#addressFull").value = p.formatted_address || ""; $("#placeId").value = p.place_id || "";
        $("#lat").value = p.geometry && p.geometry.location ? p.geometry.location.lat() : ""; $("#lng").value = p.geometry && p.geometry.location ? p.geometry.location.lng() : "";
        const comps={}; (p.address_components||[]).forEach(c=>c.types.forEach(t=>{comps[t]=c;}));
        $("#streetNumber").value = (comps.street_number && comps.street_number.long_name) || "";
        $("#route").value        = (comps.route && comps.route.long_name) || "";
        $("#locality").value     = (comps.locality && comps.locality.long_name) || (comps.sublocality && comps.sublocality.long_name) || "";
        $("#state").value        = (comps.administrative_area_level_1 && comps.administrative_area_level_1.short_name) || "";
        $("#addrPostcode").value = (comps.postal_code && comps.postal_code.long_name) || "";
        $("#address").value = $("#addressFull").value;

        save("step"); showStep(1); // exit hero mode to step 2 UI
      });
    };

    // Start
    showStep(0);
  })();
  </script>

  <!-- Google Places (key embedded as requested) -->
  <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBpch_gAy-hFApqu4wVX7X42HqFR4qYMoY&libraries=places&callback=initPlaces" async defer></script>
</body>
</html>
