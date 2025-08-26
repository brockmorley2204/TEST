<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Find Your Top Local Agent</title>
  <style>
    :root{--brand:#0b66ff;--brand2:#6aa5ff;--ink:#182338;--muted:#6b768a;--ok:#2e7d32;--line:rgba(24,35,56,.10)}
    *{box-sizing:border-box}
    html,body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;background:transparent;color:var(--ink)}
    .wrap{max-width:680px;margin:44px auto;padding:0 18px}
    .card{background:rgba(255,255,255,.72);border:1px solid rgba(255,255,255,.45);border-radius:18px;padding:28px 26px 24px;
      backdrop-filter:saturate(1.1) blur(3px);-webkit-backdrop-filter:saturate(1.1) blur(3px);box-shadow:0 24px 60px -30px rgba(24,35,56,.35)}
    .hidden{display:none!important}
    .center{text-align:center}

    /* progress (hidden on step 1) */
    .stepper{display:flex;gap:10px;justify-content:center;margin:4px 0 10px;padding:0;list-style:none}
    .dot{width:26px;height:26px;border-radius:50%;display:grid;place-items:center;font-weight:800;font-size:12px;color:#5f6b85;background:#eef2ff;border:1px solid #d9e4ff}
    .stepper li.active .dot{background:var(--brand);border-color:var(--brand);color:#fff;box-shadow:0 6px 16px rgba(11,102,255,.35)}
    .progress{height:8px;background:#eef2f7;border-radius:999px;overflow:hidden;margin:8px 0 18px}
    .bar{height:100%;width:0;background:linear-gradient(90deg,var(--brand),var(--brand2));transition:width .2s ease}

    .step{display:none;animation:fade .18s ease}
    .step.active{display:block}
    @keyframes fade{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:translateY(0)}}

    label{display:block;margin:10px 0 8px;font-weight:800}
    input,select{width:100%;padding:14px;border:1px solid var(--line);border-radius:12px;font-size:15px;background:#fff;transition:border .12s,box-shadow .12s}
    input:focus{outline:none;border-color:#c9d7ff;box-shadow:0 0 0 4px #eaf0ff}

    /* Search-style address bar */
    .searchwrap{position:relative;margin-top:6px}
    .searchbar{padding:16px 52px 16px 48px;border-radius:999px;font-size:16px;border:1px solid #dce3f0;background:#fff}
    .searchicon{position:absolute;left:14px;top:50%;transform:translateY(-50%);font-size:18px;opacity:.7}
    .kbd{position:absolute;right:14px;top:50%;transform:translateY(-50%);font-size:12px;color:#95a0b6;border:1px solid #dbe1ee;border-radius:6px;padding:3px 6px;background:#f7f9ff}
    .powered{font-size:10px;color:#9aa6bf;margin-top:8px}

    .choices{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px;margin-top:6px}
    .choice{display:flex;align-items:center;justify-content:center;text-align:center;padding:14px 10px;border:1px solid var(--line);border-radius:14px;background:#fff;font-weight:800;user-select:none;cursor:pointer;transition:transform .05s,box-shadow .12s,border-color .12s,background .12s}
    .choice:hover{border-color:#cfd6e3;box-shadow:0 10px 18px -12px rgba(24,35,56,.25)}
    .choice:active{transform:translateY(1px)}
    .choice.selected{outline:2px solid var(--brand);background:#f5f8ff}
    @media (max-width:520px){.choices{grid-template-columns:1fr}}

    .btns{display:flex;gap:12px;margin-top:16px}
    button{cursor:pointer;border:0;padding:12px 16px;border-radius:12px;font-weight:800;letter-spacing:.2px}
    .next{background:var(--brand);color:#fff;box-shadow:0 10px 22px -10px rgba(11,102,255,.6)}
    .back{background:#eef2f7;color:#2b3c55}
    .submit{background:var(--ok);color:#fff;box-shadow:0 10px 22px -12px rgba(46,125,50,.5)}

    .tiny{font-size:12px;color:#7f8aa1;margin-top:10px}
    .err{color:#b00020;margin-top:6px;display:none}
    #status{margin-top:8px;min-height:16px;color:#6b768a}
    #status.saved{color:#2e7d32}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="card">

      <!-- stepper/progress (hidden on step 1 for clean hero) -->
      <div id="controlsWrap">
        <ol class="stepper" id="stepper" aria-label="Form progress">
          <li class="active"><span class="dot">1</span></li>
          <li><span class="dot">2</span></li>
          <li><span class="dot">3</span></li>
          <li><span class="dot">4</span></li>
          <li><span class="dot">5</span></li>
        </ol>
        <div class="progress"><div class="bar" id="bar"></div></div>
      </div>

      <form id="funnel" novalidate>
        <!-- 1) ADDRESS (search-first) -->
        <div class="step active" data-step="1">
          <label for="address">Search your property address</label>
          <div class="searchwrap">
            <span class="searchicon">🔎</span>
            <input id="address" name="address" class="searchbar" placeholder="Start typing your address…"
                   autocomplete="street-address" required />
            <span class="kbd">Enter</span>
          </div>
          <div class="powered">Powered by Google</div>

          <!-- Hidden structured fields -->
          <input type="hidden" id="addressFull"   name="addressFull" />
          <input type="hidden" id="placeId"       name="placeId" />
          <input type="hidden" id="streetNumber"  name="streetNumber" />
          <input type="hidden" id="route"         name="route" />
          <input type="hidden" id="locality"      name="locality" />
          <input type="hidden" id="state"         name="state" />
          <input type="hidden" id="addrPostcode"  name="addressPostcode" />
          <input type="hidden" id="lat"           name="lat" />
          <input type="hidden" id="lng"           name="lng" />

          <div id="addrErr" class="err">Please select an address from the list, or use manual entry.</div>
          <div class="tiny" style="margin-top:8px">
            <a href="#" id="manualToggle">Can’t find it? Enter address manually.</a>
          </div>

          <!-- Manual fallback -->
          <div id="manualBlock" style="display:none;margin-top:10px">
            <div style="display:grid;gap:10px;grid-template-columns:1fr">
              <input id="m_street"  name="m_street"  placeholder="Street address (e.g. 10 Example St)" />
              <input id="m_suburb"  name="m_suburb"  placeholder="Suburb" />
              <div style="display:grid;gap:10px;grid-template-columns:2fr 1fr">
                <input id="m_state"   name="m_state"   placeholder="State (e.g. NSW)" />
                <input id="m_postcode" name="m_postcode" inputmode="numeric" maxlength="4" placeholder="Postcode" />
              </div>
            </div>
            <div class="tiny">Manual entry accepted if autocomplete isn’t available.</div>
          </div>

          <div class="btns">
            <!-- Back hidden on first step; Next only used for manual mode -->
            <button type="button" id="addrNext" class="next" style="display:none">Next</button>
          </div>
        </div>

        <!-- 2) PROPERTY TYPE -->
        <div class="step" data-step="2">
          <label>Property type</label>
          <input type="hidden" id="ptypeHidden" name="propertyType" required />
          <div class="choices" id="ptypeChoices">
            <div class="choice" data-value="House">🏠 House</div>
            <div class="choice" data-value="Unit">🏢 Unit</div>
            <div class="choice" data-value="Townhouse">🏘️ Townhouse</div>
            <div class="choice" data-value="Land">🧱 Land</div>
            <div class="choice" data-value="Other">🧩 Other</div>
          </div>
          <div class="btns" style="margin-top:12px"><button type="button" class="back">Back</button></div>
        </div>

        <!-- 3) PRICE -->
        <div class="step" data-step="3">
          <label>Estimated price point</label>
          <input type="hidden" id="priceBandHidden" name="priceBand" required />
          <div class="choices" id="priceChoices">
            <div class="choice" data-value="0–500k">0–500k</div>
            <div class="choice" data-value="500k–1m">500k–1m</div>
            <div class="choice" data-value="$1m–$2m">$1m–$2m</div>
            <div class="choice" data-value="$2m+">$2m+</div>
          </div>
          <div class="btns" style="margin-top:12px"><button type="button" class="back">Back</button></div>
        </div>

        <!-- 4) PHONE -->
        <div class="step" data-step="4">
          <label for="phone">Best phone number</label>
          <input id="phone" name="phone" inputmode="tel" maxlength="20" placeholder="e.g. 0400 000 000" required />
          <div class="tiny">We’ll only use this to share your short-list and next steps.</div>
          <div class="btns"><button type="button" class="back">Back</button><button type="button" class="next">Next</button></div>
        </div>

        <!-- 5) EMAIL -->
        <div class="step" data-step="5">
          <label for="email">Email for your results</label>
          <input id="email" name="email" type="email" inputmode="email" autocomplete="email" placeholder="you@example.com" required />
          <div class="tiny">By continuing, you agree we may contact you about your appraisal. You can opt out anytime.</div>
          <div class="btns"><button type="button" class="back">Back</button><button type="submit" class="submit">See my match</button></div>
        </div>

        <!-- 6) THANK YOU -->
        <div class="step" data-step="6">
          <div class="center" style="padding:10px 2px">
            <h2 style="margin:6px 0 10px">Thank you for completing! 🎉</h2>
            <p style="margin:0 0 10px;color:var(--muted)">We will send you the match very shortly!</p>
            <p class="tiny">Keep an eye on your email and phone for the details.</p>
          </div>
        </div>
      </form>

      <div id="status" class="tiny" aria-live="polite"></div>
    </div>
  </div>

  <script>
  (function(){
    // ===== CONFIG =====
    const WEBHOOK_URL = "https://script.google.com/macros/s/AKfycbwQW0E9h0gWIN-wX6CDKUQJrjXzkfgA6sTIpqwd3-HisuiiJGXgcgSsUl5HWH8uV48/exec";
    const PROJECT = "agent-matcher-v1";
    const formSteps = 5; // Address, Type, Price, Phone, Email

    // ===== UTIL =====
    const $ = s => document.querySelector(s);
    function readParams(){
      const url = new URL(window.location.href);
      const keys = ["gclid","utm_source","utm_medium","utm_campaign","utm_term","utm_content","fbclid","msclkid"];
      const out = {}; keys.forEach(k => { const v = url.searchParams.get(k); if (v) out[k] = v; });
      return out;
    }
    const CAMPAIGN = (() => {
      const key = PROJECT + ":campaign";
      try{
        const existing = sessionStorage.getItem(key);
        if (existing) return JSON.parse(existing);
        const fresh = readParams();
        sessionStorage.setItem(key, JSON.stringify(fresh));
        return fresh;
      }catch(_){ return readParams(); }
    })();
    const leadId = (()=>{ const k=PROJECT+":leadId"; let id=localStorage.getItem(k);
      if(!id){ id=(crypto.randomUUID?crypto.randomUUID():String(Date.now())+Math.random().toString(16).slice(2)); localStorage.setItem(k,id); }
      return id;
    })();

    // ===== ELEMENTS =====
    const form = $("#funnel");
    const steps = [...form.querySelectorAll(".step")];
    const bar = $("#bar");
    const statusEl = $("#status");
    const stepperEls = $("#stepper") ? Array.from($("#stepper").children) : [];
    const controlsWrap = $("#controlsWrap");
    const addrErr = $("#addrErr");
    const manualToggle = $("#manualToggle");
    const manualBlock = $("#manualBlock");
    const addrNextBtn = $("#addrNext");
    let stepIndex = 0;
    let addressSelected = false;
    let manualMode = false;

    // UI helpers
    function updateUI(){
      steps.forEach((s,i)=>s.classList.toggle("active", i===stepIndex));
      const pct = Math.min(100, Math.round((Math.min(stepIndex+1, formSteps)/formSteps)*100));
      if (bar) bar.style.width = pct + "%";
      if (stepperEls.length) stepperEls.forEach((li,i)=>li.classList.toggle("active", i===Math.min(stepIndex, formSteps-1)));
      if (controlsWrap) controlsWrap.classList.toggle("hidden", stepIndex===0);
      if (statusEl && stepIndex===0) statusEl.textContent = "";
    }
    function showStep(i){ stepIndex=Math.max(0,Math.min(i,steps.length-1)); updateUI(); }

    function getCurrentFields(){
      const step = steps[stepIndex];
      const inputs = step ? step.querySelectorAll("input, select") : [];
      const data = {};
      inputs.forEach(inp=>{
        let v = (inp.value || "").trim();
        data[inp.name || inp.id] = v;
      });
      return data;
    }

    // Validation (special case for step 1 address)
    function validForStep(){
      const step = steps[stepIndex];
      if (!step) return true;

      if (stepIndex === 0) {
        if (manualMode) {
          const street = ($("#m_street").value||"").trim();
          const suburb = ($("#m_suburb").value||"").trim();
          const state  = ($("#m_state").value||"").trim();
          const pc     = ($("#m_postcode").value||"").trim();
          if (!street || !suburb || !state || pc.length !== 4) return false;
          // Copy manual into visible + structured
          $("#address").value = `${street}, ${suburb} ${state} ${pc}`;
          $("#addressFull").value = $("#address").value;
          $("#placeId").value = ""; $("#streetNumber").value=""; $("#route").value="";
          $("#locality").value = suburb; $("#state").value = state; $("#addrPostcode").value = pc;
          $("#lat").value = ""; $("#lng").value = "";
          if (addrErr) addrErr.style.display = "none";
          return true;
        } else {
          if (!addressSelected || !$("#placeId").value) {
            if (addrErr) addrErr.style.display = "block";
            return false;
          }
        }
      }

      // Generic required checks
      const inputs = step.querySelectorAll("input[required], select[required]");
      for(const inp of inputs){
        let v = (inp.value || "").trim();
        if(!v) return false;
        if(inp.type==="email"){
          inp.value = v;
          if (typeof inp.checkValidity==="function" && !inp.checkValidity()) return false;
          if (!inp.checkValidity && !/.+@.+\..+/.test(v)) return false;
        }
      }
      return true;
    }

    // Save each step to your Apps Script
    function savePartial(eventType){
      const payload = {
        leadId,
        event: eventType,
        stepNumber: stepIndex + 1,
        answers: getCurrentFields(),
        tracking: { ...CAMPAIGN, landing: location.pathname + location.search, referrer: document.referrer || null },
        userAgent: navigator.userAgent,
        screen: { w: innerWidth, h: innerHeight, dpr: devicePixelRatio || 1 },
        ts: new Date().toISOString()
      };
      const json = JSON.stringify(payload);
      let sent=false;
      try{ fetch(WEBHOOK_URL,{method:"POST",mode:"no-cors",body:json,keepalive:true}); sent=true; }
      catch(e){ try{ if(navigator.sendBeacon){ const b=new Blob([json],{type:"text/plain"}); navigator.sendBeacon(WEBHOOK_URL,b); sent=true; } }catch(_){ } }
      if (statusEl && stepIndex>0) { statusEl.textContent = sent ? "Saved." : "Working offline—will retry on next step."; statusEl.classList.toggle("saved", sent); }
      return payload;
    }

    function tryAdvance(){
      if(!validForStep()){ if(statusEl){ statusEl.textContent="Please complete this step."; statusEl.classList.remove("saved"); } return; }
      savePartial("step"); showStep(stepIndex+1);
    }

    // Manual toggle & behavior
    if (manualToggle) manualToggle.addEventListener("click", (e)=>{
      e.preventDefault();
      manualMode = !manualMode;
      manualBlock.style.display = manualMode ? "block" : "none";
      addrNextBtn.style.display = manualMode ? "inline-block" : "none";
      addressSelected = false;
      $("#placeId").value = "";
      if (addrErr) addrErr.style.display = "none";
    });

    // Address input behavior
    const addrInput = $("#address");
    if (addrInput) {
      addrInput.addEventListener("input", ()=>{
        addressSelected = false;
        $("#placeId").value = "";
        if (addrErr) addrErr.style.display = "none";
      });
      addrInput.addEventListener("keydown",(e)=>{
        if(e.key==="Enter"){
          if (manualMode) { e.preventDefault(); tryAdvance(); }
          else { e.preventDefault(); /* must pick from suggestions */ }
        }
      });
    }

    // Click handlers
    form.addEventListener("click",(e)=>{
      const el = e.target.closest && e.target.closest(".choice, .next, .back");
      if(!el) return;
      if(el.classList.contains("back")){ showStep(stepIndex-1); return; }
      if(el.id === "addrNext"){ tryAdvance(); return; }
      if(el.classList.contains("next")){ tryAdvance(); return; }
      if(el.classList.contains("choice")){
        const group = el.parentElement;
        group.querySelectorAll(".choice").forEach(c=>c.classList.remove("selected"));
        el.classList.add("selected");
        if(group.id==="ptypeChoices"){ $("#ptypeHidden").value = el.dataset.value; }
        if(group.id==="priceChoices"){ $("#priceBandHidden").value = el.dataset.value; }
        savePartial("step"); showStep(stepIndex+1);
      }
    });

    // Submit (final)
    form.addEventListener("submit",(e)=>{
      e.preventDefault();
      if(!validForStep()){ if(statusEl){ statusEl.textContent="Please complete this step."; statusEl.classList.remove("saved"); } return; }
      const payload = savePartial("complete");
      window.dataLayer = window.dataLayer || [];
      window.dataLayer.push({ event:"lead_complete", leadId: payload.leadId, postcode: $("#addrPostcode").value || null });
      showStep(formSteps); // thank-you
    });

    // Init UI
    updateUI();

    // ===== Expose Places init =====
    window.initPlaces = function(){
      const input = document.getElementById("address");
      if (!window.google || !google.maps || !google.maps.places || !input) {
        if (addrNextBtn) addrNextBtn.style.display = "inline-block"; // allow manual path if Places fails
        return;
      }

      const ac = new google.maps.places.Autocomplete(input, {
        types: ["address"],
        componentRestrictions: { country: "au" },
        fields: ["address_components","formatted_address","geometry","place_id"]
      });

      ac.addListener("place_changed", ()=>{
        const place = ac.getPlace();
        if (!place || !place.address_components) return;
        addressSelected = true;
        if (addrErr) addrErr.style.display = "none";

        // Fill hidden fields
        $("#addressFull").value = place.formatted_address || "";
        $("#placeId").value = place.place_id || "";
        $("#lat").value = place.geometry && place.geometry.location ? place.geometry.location.lat() : "";
        $("#lng").value = place.geometry && place.geometry.location ? place.geometry.location.lng() : "";

        // Parse components
        const comps = {};
        (place.address_components || []).forEach(c=>c.types.forEach(t=>{ comps[t]=c; }));
        $("#streetNumber").value = (comps.street_number && comps.street_number.long_name) || "";
        $("#route").value        = (comps.route && comps.route.long_name) || "";
        $("#locality").value     = (comps.locality && comps.locality.long_name) || (comps.sublocality && comps.sublocality.long_name) || "";
        $("#state").value        = (comps.administrative_area_level_1 && comps.administrative_area_level_1.short_name) || "";
        $("#addrPostcode").value = (comps.postal_code && comps.postal_code.long_name) || "";

        // Ensure visible input is formatted address
        $("#address").value = $("#addressFull").value;

        // Auto-advance
        setTimeout(()=>{ savePartial("step"); showStep(1); }, 80);
      });
    };
  })();
  </script>

  <!-- Google Places (replace YOUR_GOOGLE_MAPS_API_KEY) -->
  <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_MAPS_API_KEY&libraries=places&callback=initPlaces" async defer></script>
</body>
</html>
