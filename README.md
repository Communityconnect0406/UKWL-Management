






<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>UKWL Staff Dashboard</title>
  <style>
    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top left, #1f2933, #020617);
      color: #e5e7eb;
    }
    .centered {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }
    .card {
      background: rgba(15, 23, 42, 0.85);
      border-radius: 16px;
      padding: 24px 28px;
      box-shadow: 0 18px 45px rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(18px);
      border: 1px solid rgba(148, 163, 184, 0.35);
      max-width: 900px;
      width: 100%;
    }
    h1, h2, h3 {
      margin-top: 0;
      color: #f9fafb;
    }
    input, select, button, textarea {
      font-family: inherit;
    }
    .input-group {
      margin-bottom: 12px;
    }
    .input-group label {
      display: block;
      font-size: 0.85rem;
      margin-bottom: 4px;
      color: #cbd5f5;
    }
    .input-group input,
    .input-group select,
    .input-group textarea {
      width: 100%;
      padding: 8px 10px;
      border-radius: 8px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: rgba(15, 23, 42, 0.9);
      color: #e5e7eb;
      font-size: 0.9rem;
      outline: none;
    }
    .input-group textarea {
      resize: vertical;
      min-height: 70px;
    }
    .tabs {
      display: flex;
      gap: 8px;
      margin-bottom: 16px;
      border-bottom: 1px solid rgba(148, 163, 184, 0.35);
      padding-bottom: 8px;
    }
    .tab-btn {
      padding: 8px 14px;
      border-radius: 999px;
      border: 1px solid transparent;
      background: transparent;
      color: #9ca3af;
      cursor: pointer;
      font-size: 0.9rem;
    }
    .tab-btn.active {
      background: rgba(59, 130, 246, 0.15);
      border-color: rgba(59, 130, 246, 0.7);
      color: #e5e7eb;
    }
    .tab-content {
      display: none;
    }
    .tab-content.active {
      display: block;
    }
    .session-types {
      display: flex;
      gap: 8px;
      margin-top: 12px;
      flex-wrap: wrap;
    }
    .session-type-btn {
      flex: 1;
      min-width: 140px;
      padding: 8px 10px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: rgba(15, 23, 42, 0.9);
      color: #e5e7eb;
      cursor: pointer;
      font-size: 0.85rem;
    }
    .session-type-btn.active {
      border-color: rgba(59, 130, 246, 0.9);
      background: rgba(37, 99, 235, 0.25);
    }
    .actions {
      margin-top: 16px;
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      align-items: center;
    }
    .btn-primary {
      padding: 9px 16px;
      border-radius: 999px;
      border: none;
      background: linear-gradient(135deg, #3b82f6, #6366f1);
      color: #f9fafb;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.9rem;
    }
    .btn-primary:disabled {
      opacity: 0.6;
      cursor: default;
    }
    .status {
      font-size: 0.8rem;
      color: #9ca3af;
    }
    .password-input {
      width: 100%;
      padding: 10px 12px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: rgba(15, 23, 42, 0.9);
      color: #e5e7eb;
      font-size: 0.95rem;
      outline: none;
      margin-bottom: 10px;
    }
    .error {
      color: #f97373;
      font-size: 0.8rem;
      margin-top: 4px;
    }
  </style>
</head>
<body>
  <!-- Password Gate -->
  <div id="login-screen" class="centered">
    <div class="card">
      <h2>UKWL Staff Dashboard</h2>
      <p style="font-size:0.9rem;color:#9ca3af;margin-bottom:14px;">
        Enter the staff password to access session tools, infractions, and promotions.
      </p>
      <input id="password-input" type="password" class="password-input" placeholder="Password" />
      <button id="password-submit" class="btn-primary">Enter</button>
      <div id="password-error" class="error" style="display:none;">Incorrect password.</div>
    </div>
  </div>

  <!-- Main Dashboard -->
  <div id="dashboard" style="display:none;">
    <div class="centered" style="align-items:flex-start;padding:32px 16px;">
      <div class="card">
        <h1 style="font-size:1.4rem;margin-bottom:4px;">UKWL Staff Dashboard</h1>
        <p style="font-size:0.85rem;color:#9ca3af;margin-top:0;margin-bottom:18px;">
          Sessions, staff infractions, and promotions—sent directly via Discord webhooks.
        </p>

        <div class="tabs">
          <button class="tab-btn active" data-tab="sessions">Sessions</button>
          <button class="tab-btn" data-tab="infractions">Staff Infractions</button>
          <button class="tab-btn" data-tab="promotions">Staff Promotions</button>
        </div>

        <!-- Sessions Tab -->
        <div id="tab-sessions" class="tab-content active">
          <h3>Session announcement</h3>
          <p style="font-size:0.85rem;color:#9ca3af;">
            Select the session type below. It will automatically ping <code>@everyone</code>.
          </p>

          <div class="session-types">
            <button class="session-type-btn active" data-type="startup">Session start up</button>
            <button class="session-type-btn" data-type="vote">Session vote</button>
            <button class="session-type-btn" data-type="shutdown">Server shutdown</button>
          </div>

          <div class="input-group" style="margin-top:14px;">
            <label>Preview (read-only)</label>
            <textarea id="session-preview" readonly></textarea>
          </div>

          <div class="actions">
            <span id="session-status" class="status"></span>
            <button id="session-send" class="btn-primary">Send session embed</button>
          </div>
        </div>

        <!-- Infractions Tab -->
        <div id="tab-infractions" class="tab-content">
          <h3>Staff infractions</h3>
          <p style="font-size:0.85rem;color:#9ca3af;">
            IDs are used only to ping in the message content; they will not be shown as plain text.
          </p>

          <div class="input-group">
            <label>Staff member ID (will be pinged)</label>
            <input id="infraction-staff-id" type="text" placeholder="e.g. 123456789012345678" />
          </div>

          <div class="input-group">
            <label>Infraction type</label>
            <select id="infraction-type">
              <option value="Verbal warning 1">Verbal warning 1</option>
              <option value="Warning 2">Warning 2</option>
              <option value="Strike 1">Strike 1</option>
              <option value="Strike 2">Strike 2</option>
              <option value="Suspension">Suspension</option>
              <option value="Termination">Termination</option>
              <option value="Blacklist">Blacklist</option>
            </select>
          </div>

          <div class="input-group">
            <label>Issuer ID (will be pinged)</label>
            <input id="infraction-issuer-id" type="text" placeholder="e.g. 123456789012345678" />
          </div>

          <div class="input-group">
            <label>Reason (required)</label>
            <textarea id="infraction-reason" placeholder="Explain the reason for this infraction"></textarea>
          </div>

          <div class="input-group">
            <label>Notes (required)</label>
            <textarea id="infraction-notes" placeholder="Any additional notes or context"></textarea>
          </div>

          <div class="input-group">
            <label>Preview (read-only)</label>
            <textarea id="infraction-preview" readonly></textarea>
          </div>

          <div class="actions">
            <span id="infraction-status" class="status"></span>
            <button id="infraction-send" class="btn-primary">Send infraction embed</button>
          </div>
        </div>

        <!-- Promotions Tab -->
        <div id="tab-promotions" class="tab-content">
          <h3>Staff promotions</h3>
          <p style="font-size:0.85rem;color:#9ca3af;">
            Use IDs and role IDs in ping format (e.g. <code>&lt;@123...&gt;</code>, <code>&lt;@&amp;roleId&gt;</code>).
          </p>

          <div class="input-group">
            <label>Staff member ID (will be pinged)</label>
            <input id="promo-staff-id" type="text" placeholder="e.g. 123456789012345678" />
          </div>

          <div class="input-group">
            <label>New rank (role ping, e.g. &lt;@&amp;1489797691587297582&gt;)</label>
            <input id="promo-rank" type="text" placeholder="<@&1489797691587297582>" />
          </div>

          <div class="input-group">
            <label>Issuer ID (will be pinged)</label>
            <input id="promo-issuer-id" type="text" placeholder="e.g. 123456789012345678" />
          </div>

          <div class="input-group">
            <label>Reason (required)</label>
            <textarea id="promo-reason" placeholder="Why is this promotion being given?"></textarea>
          </div>

          <div class="input-group">
            <label>Notes (required)</label>
            <textarea id="promo-notes" placeholder="Any additional notes or context"></textarea>
          </div>

          <div class="input-group">
            <label>Preview (read-only)</label>
            <textarea id="promo-preview" readonly></textarea>
          </div>

          <div class="actions">
            <span id="promo-status" class="status"></span>
            <button id="promo-send" class="btn-primary">Send promotion embed</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    // ===== Password Gate =====
    const PASSWORD = "UKWL_040608";
    const loginScreen = document.getElementById("login-screen");
    const dashboard = document.getElementById("dashboard");
    const passwordInput = document.getElementById("password-input");
    const passwordSubmit = document.getElementById("password-submit");
    const passwordError = document.getElementById("password-error");

    passwordSubmit.addEventListener("click", () => {
      if (passwordInput.value === PASSWORD) {
        loginScreen.style.display = "none";
        dashboard.style.display = "block";
      } else {
        passwordError.style.display = "block";
      }
    });

    passwordInput.addEventListener("keydown", (e) => {
      if (e.key === "Enter") passwordSubmit.click();
    });

    // ===== Tabs =====
    const tabButtons = document.querySelectorAll(".tab-btn");
    const tabContents = {
      sessions: document.getElementById("tab-sessions"),
      infractions: document.getElementById("tab-infractions"),
      promotions: document.getElementById("tab-promotions"),
    };

    tabButtons.forEach((btn) => {
      btn.addEventListener("click", () => {
        tabButtons.forEach((b) => b.classList.remove("active"));
        btn.classList.add("active");
        const tab = btn.dataset.tab;
        Object.keys(tabContents).forEach((key) => {
          tabContents[key].classList.toggle("active", key === tab);
        });
      });
    });

    // ===== Webhook URLs =====
    const SESSIONS_WEBHOOK = "https://discord.com/api/webhooks/1489994575865974834/-VwBsMyMWoVn8wega237LSt-gqeVvrU-M-27T1lz8yr5Pz0GjEjLZWw7d_1XxMIaCZdW";
    const INFRACTIONS_WEBHOOK = "https://discord.com/api/webhooks/1489997587770511664/I8RyanlzWjGsD5hvOGOarKO7cbOdvMP-gus_99Qp8fOM1M2oDRZ8DBr3SxhauE9WHSW8";
    const PROMOTIONS_WEBHOOK = "https://discord.com/api/webhooks/1489997472628605061/2lu_HLyeTzz74TRM4vtBbYSIWs9fpT_VMB6TYUzEGauX0TdiTD51C8uaXH5Gn62BF8mR";

    // ===== Sessions Logic =====
    const sessionButtons = document.querySelectorAll(".session-type-btn");
    const sessionPreview = document.getElementById("session-preview");
    const sessionStatus = document.getElementById("session-status");
    const sessionSend = document.getElementById("session-send");

    const sessionThumbnail = "https://media.discordapp.net/attachments/1489805815559880937/1489995633136898068/Copilot_20260404_150901.png?ex=69d27211&is=69d12091&hm=0a258095b80debfed0f238bf4eda17c455f8615fafe71a68ac29dd3b529fb951&=&format=webp&quality=lossless&width=602&height=401";

    const sessionTemplates = {
      startup: {
        title: "Session Start",
        description:
          "Hello UKWL, a session has started! Join our community server now in ERLC for some amazing roleplays.\n\nCode: `UKWL0802`",
      },
      vote: {
        title: "Session Vote",
        description:
          "Hello UKWL, This is a proposed session notice, and we need to reach 4 votes before hosting can happen! Be sure to cast your vote and join the fun.",
      },
      shutdown: {
        title: "Server Shutdown",
        description:
          "Thanks for joining our role play today, UKWL. Be sure to return tomorrow or later when the session resumes.",
      },
    };

    let currentSessionType = "startup";

    function updateSessionPreview() {
      const t = sessionTemplates[currentSessionType];
      sessionPreview.value = `@everyone\n\n${t.title}\n\n${t.description}`;
    }

    sessionButtons.forEach((btn) => {
      btn.addEventListener("click", () => {
        sessionButtons.forEach((b) => b.classList.remove("active"));
        btn.classList.add("active");
        currentSessionType = btn.dataset.type;
        updateSessionPreview();
      });
    });

    updateSessionPreview();

    sessionSend.addEventListener("click", async () => {
      const template = sessionTemplates[currentSessionType];
      sessionStatus.textContent = "Sending...";
      sessionSend.disabled = true;

      const payload = {
        content: "@everyone",
        embeds: [
          {
            title: template.title,
            description: template.description,
            color: 0x3b82f6,
            thumbnail: {
              url: sessionThumbnail,
            },
          },
        ],
      };

      try {
        const res = await fetch(SESSIONS_WEBHOOK, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload),
        });
        if (!res.ok) throw new Error("Request failed");
        sessionStatus.textContent = "Session announcement sent.";
      } catch (err) {
        sessionStatus.textContent = "Error sending session announcement.";
      } finally {
        sessionSend.disabled = false;
        setTimeout(() => (sessionStatus.textContent = ""), 4000);
      }
    });

    // ===== Infractions Logic =====
    const infractionStaffId = document.getElementById("infraction-staff-id");
    const infractionType = document.getElementById("infraction-type");
    const infractionIssuerId = document.getElementById("infraction-issuer-id");
    const infractionReason = document.getElementById("infraction-reason");
    const infractionNotes = document.getElementById("infraction-notes");
    const infractionPreview = document.getElementById("infraction-preview");
    const infractionStatus = document.getElementById("infraction-status");
    const infractionSend = document.getElementById("infraction-send");

    const infractionThumbnail = "https://media.discordapp.net/attachments/1489805815559880937/1489996522757029948/Copilot_20260404_151820.png?ex=69d272e5&is=69d12165&hm=bd99e4014ab6013c80ff0f90071604b7967ec4ef2674b04d457c195c69e02425&=&format=webp&quality=lossless&width=1272&height=848";

    function buildInfractionText() {
      const staffPing = infractionStaffId.value ? `<@${infractionStaffId.value}>` : "<@staff>";
      const issuerPing = infractionIssuerId.value ? `<@${infractionIssuerId.value}>` : "<@issuer>";
      const type = infractionType.value || "Infraction";
      const reason = infractionReason.value || "(no reason provided)";
      const notes = infractionNotes.value || "(no notes provided)";

      return (
        `Dear ${staffPing} Due to your recent actions, you have received an infraction (${type}). ` +
        `If you believe this is incorrect, please open an internal affairs support ticket.\n` +
        `https://discord.com/channels/1489790170059374692/1489805969683648603\n\n` +
        `Staff member: ${staffPing}\n` +
        `Infraction type: ${type}\n` +
        `Issuer: ${issuerPing}\n` +
        `Reason: ${reason}\n` +
        `Notes: ${notes}`
      );
    }

    function updateInfractionPreview() {
      infractionPreview.value = buildInfractionText();
    }

    [infractionStaffId, infractionType, infractionIssuerId, infractionReason, infractionNotes].forEach((el) =>
      el.addEventListener("input", updateInfractionPreview)
    );
    updateInfractionPreview();

    infractionSend.addEventListener("click", async () => {
      const staffId = infractionStaffId.value.trim();
      const issuerId = infractionIssuerId.value.trim();
      const reason = infractionReason.value.trim();
      const notes = infractionNotes.value.trim();

      if (!staffId || !issuerId || !reason || !notes) {
        infractionStatus.textContent = "Staff ID, issuer ID, reason, and notes are required.";
        setTimeout(() => (infractionStatus.textContent = ""), 4000);
        return;
      }

      const staffPing = `<@${staffId}>`;
      const issuerPing = `<@${issuerId}>`;
      const type = infractionType.value;
      const description = buildInfractionText();

      infractionStatus.textContent = "Sending...";
      infractionSend.disabled = true;

      const payload = {
        content: `${staffPing} ${issuerPing}`,
        embeds: [
          {
            title: `Staff Infraction - ${type}`,
            description: description,
            color: 0xf97316,
            thumbnail: {
              url: infractionThumbnail,
            },
          },
        ],
      };

      try {
        const res = await fetch(INFRACTIONS_WEBHOOK, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload),
        });
        if (!res.ok) throw new Error("Request failed");
        infractionStatus.textContent = "Infraction sent.";
      } catch (err) {
        infractionStatus.textContent = "Error sending infraction.";
      } finally {
        infractionSend.disabled = false;
        setTimeout(() => (infractionStatus.textContent = ""), 4000);
      }
    });

    // ===== Promotions Logic =====
    const promoStaffId = document.getElementById("promo-staff-id");
    const promoRank = document.getElementById("promo-rank");
    const promoIssuerId = document.getElementById("promo-issuer-id");
    const promoReason = document.getElementById("promo-reason");
    const promoNotes = document.getElementById("promo-notes");
    const promoPreview = document.getElementById("promo-preview");
    const promoStatus = document.getElementById("promo-status");
    const promoSend = document.getElementById("promo-send");

    const promoThumbnail = "https://media.discordapp.net/attachments/1489806195165237440/1489996980229771395/Copilot_20260404_151347.png?ex=69d27352&is=69d121d2&hm=f12bc3a3419293f5917a508f6965e74f4575af9857a2f2f58eb258f9889aa0f9&=&format=webp&quality=lossless&width=602&height=401";

    function buildPromoText() {
      const staffPing = promoStaffId.value ? `<@${promoStaffId.value}>` : "<@staff>";
      const issuerPing = promoIssuerId.value ? `<@${promoIssuerId.value}>` : "<@issuer>";
      const rank = promoRank.value || "<new rank>";
      const reason = promoReason.value || "(no reason provided)";
      const notes = promoNotes.value || "(no notes provided)";

      return (
        `Congratulations ${staffPing} you have received a promotion for your recent work in our community. ` +
        `Keep up this work! If you need any support with this change go ahead and open a support ticket!\n` +
        `https://discord.com/channels/1489790170059374692/1489805969683648603\n\n` +
        `Staff member: ${staffPing}\n` +
        `New Rank: ${rank}\n` +
        `Issuer: ${issuerPing}\n` +
        `Reason: ${reason}\n` +
        `Notes: ${notes}`
      );
    }

    function updatePromoPreview() {
      promoPreview.value = buildPromoText();
    }

    [promoStaffId, promoRank, promoIssuerId, promoReason, promoNotes].forEach((el) =>
      el.addEventListener("input", updatePromoPreview)
    );
    updatePromoPreview();

    promoSend.addEventListener("click", async () => {
      const staffId = promoStaffId.value.trim();
      const issuerId = promoIssuerId.value.trim();
      const rank = promoRank.value.trim();
      const reason = promoReason.value.trim();
      const notes = promoNotes.value.trim();

      if (!staffId || !issuerId || !rank || !reason || !notes) {
        promoStatus.textContent = "Staff ID, new rank, issuer ID, reason, and notes are required.";
        setTimeout(() => (promoStatus.textContent = ""), 4000);
        return;
      }

      const staffPing = `<@${staffId}>`;
      const issuerPing = `<@${issuerId}>`;
      const description = buildPromoText();

      promoStatus.textContent = "Sending...";
      promoSend.disabled = true;

      const payload = {
        content: `${staffPing} ${issuerPing}`,
        embeds: [
          {
            title: "Staff Promotion",
            description: description,
            color: 0x22c55e,
            thumbnail: {
              url: promoThumbnail,
            },
          },
        ],
      };

      try {
        const res = await fetch(PROMOTIONS_WEBHOOK, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload),
        });
        if (!res.ok) throw new Error("Request failed");
        promoStatus.textContent = "Promotion sent.";
      } catch (err) {
        promoStatus.textContent = "Error sending promotion.";
      } finally {
        promoSend.disabled = false;
        setTimeout(() => (promoStatus.textContent = ""), 4000);
      }
    });
  </script>
</body>
</html>
