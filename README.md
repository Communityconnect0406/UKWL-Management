


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
    .error-text {
      color: #f87171;
      font-size: 0.8rem;
      margin-top: 4px;
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
  </style>
</head>
<body>

<!-- PASSWORD SCREEN -->
<div id="login-screen" class="centered">
  <div class="card">
    <h2>UKWL Staff Dashboard</h2>
    <p style="font-size:0.9rem;color:#9ca3af;margin-bottom:14px;">
      Enter the staff password to access the dashboard.
    </p>
    <input id="password-input" type="password" class="password-input" placeholder="Password" />
    <button id="password-submit" class="btn-primary">Enter</button>
    <div id="password-error" class="error-text" style="display:none;">Incorrect password.</div>
  </div>
</div>

<!-- MAIN DASHBOARD -->
<div id="dashboard" style="display:none;">
  <div class="centered" style="align-items:flex-start;padding:32px 16px;">
    <div class="card">
      <h1 style="font-size:1.4rem;margin-bottom:4px;">UKWL Staff Dashboard</h1>

      <div class="tabs">
        <button class="tab-btn active" data-tab="sessions">Sessions</button>
        <button class="tab-btn" data-tab="infractions">Infractions</button>
        <button class="tab-btn" data-tab="promotions">Promotions</button>
      </div>

      <!-- SESSIONS TAB -->
      <div id="tab-sessions" class="tab-content active">
        <h3>Session Announcement</h3>

        <div class="session-types">
          <button class="session-type-btn active" data-type="startup">Session Start</button>
          <button class="session-type-btn" data-type="vote">Session Vote</button>
          <button class="session-type-btn" data-type="shutdown">Server Shutdown</button>
        </div>

        <div class="input-group" style="margin-top:14px;">
          <label>Preview</label>
          <textarea id="session-preview" readonly></textarea>
        </div>

        <div class="actions">
          <span id="session-status" class="status"></span>
          <button id="session-send" class="btn-primary">Send Session</button>
        </div>
      </div>

      <!-- INFRACTIONS TAB -->
      <div id="tab-infractions" class="tab-content">
        <h3>Staff Infraction</h3>

        <div class="input-group">
          <label>Staff ID</label>
          <input id="inf-staff" type="text" />
        </div>

        <div class="input-group">
          <label>Infraction Type</label>
          <select id="inf-type">
            <option>Verbal warning 1</option>
            <option>Warning 2</option>
            <option>Strike 1</option>
            <option>Strike 2</option>
            <option>Suspension</option>
            <option>Termination</option>
            <option>Blacklist</option>
          </select>
        </div>

        <div class="input-group">
          <label>Issuer ID</label>
          <input id="inf-issuer" type="text" />
        </div>

        <div class="input-group">
          <label>Reason (required)</label>
          <textarea id="inf-reason"></textarea>
        </div>

        <div class="input-group">
          <label>Notes (required)</label>
          <textarea id="inf-notes"></textarea>
        </div>

        <div class="input-group">
          <label>Preview</label>
          <textarea id="inf-preview" readonly></textarea>
        </div>

        <div class="actions">
          <span id="inf-status" class="error-text"></span>
          <button id="inf-send" class="btn-primary">Send Infraction</button>
        </div>
      </div>

      <!-- PROMOTIONS TAB -->
      <div id="tab-promotions" class="tab-content">
        <h3>Staff Promotion</h3>

        <div class="input-group">
          <label>Staff ID</label>
          <input id="pro-staff" type="text" />
        </div>

        <div class="input-group">
          <label>New Rank (role ping)</label>
          <input id="pro-rank" type="text" placeholder="<@&roleID>" />
        </div>

        <div class="input-group">
          <label>Issuer ID</label>
          <input id="pro-issuer" type="text" />
        </div>

        <div class="input-group">
          <label>Reason (required)</label>
          <textarea id="pro-reason"></textarea>
        </div>

        <div class="input-group">
          <label>Notes (required)</label>
          <textarea id="pro-notes"></textarea>
        </div>

        <div class="input-group">
          <label>Preview</label>
          <textarea id="pro-preview" readonly></textarea>
        </div>

        <div class="actions">
          <span id="pro-status" class="error-text"></span>
          <button id="pro-send" class="btn-primary">Send Promotion</button>
        </div>
      </div>

    </div>
  </div>
</div>

<script>
/* PASSWORD */
const PASSWORD = "UKWL_040608";
document.getElementById("password-submit").onclick = () => {
  if (document.getElementById("password-input").value === PASSWORD) {
    document.getElementById("login-screen").style.display = "none";
    document.getElementById("dashboard").style.display = "block";
  } else {
    document.getElementById("password-error").style.display = "block";
  }
};

/* TABS */
document.querySelectorAll(".tab-btn").forEach(btn => {
  btn.onclick = () => {
    document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");

    document.querySelectorAll(".tab-content").forEach(tab => tab.classList.remove("active"));
    document.getElementById("tab-" + btn.dataset.tab).classList.add("active");
  };
});

/* WEBHOOKS */
const WH_SESSIONS = "YOUR_WEBHOOK";
const WH_INF = "YOUR_WEBHOOK";
const WH_PRO = "YOUR_WEBHOOK";

/* SESSIONS */
const sessionTemplates = {
  startup: "Hello UKWL, a session has started!\n\nCode: `UKWL0802`",
  vote: "Hello UKWL, this is a proposed session notice. We need 4 votes!",
  shutdown: "Thanks for joining our roleplay today, UKWL."
};

let currentSession = "startup";
document.querySelectorAll(".session-type-btn").forEach(btn => {
  btn.onclick = () => {
    document.querySelectorAll(".session-type-btn").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    currentSession = btn.dataset.type;
    document.getElementById("session-preview").value = sessionTemplates[currentSession];
  };
});

/* INFRACTIONS */
function updateInfPreview() {
  const staff = document.getElementById("inf-staff").value;
  const type = document.getElementById("inf-type").value;
  const issuer = document.getElementById("inf-issuer").value;
  const reason = document.getElementById("inf-reason").value;
  const notes = document.getElementById("inf-notes").value;

  document.getElementById("inf-preview").value =
`Dear <@${staff}> you have received an infraction (${type}).

Staff Member: <@${staff}>
Infraction Type: ${type}
Issuer: <@${issuer}>
Reason: ${reason}
Notes: ${notes}`;
}

document.querySelectorAll("#inf-staff,#inf-type,#inf-issuer,#inf-reason,#inf-notes")
  .forEach(el => el.oninput = updateInfPreview);

/* PROMOTIONS */
function updateProPreview() {
  const staff = document.getElementById("pro-staff").value;
  const rank = document.getElementById("pro-rank").value;
  const issuer = document.getElementById("pro-issuer").value;
  const reason = document.getElementById("pro-reason").value;
  const notes = document.getElementById("pro-notes").value;

  document.getElementById("pro-preview").value =
`Congratulations <@${staff}> you have received a promotion for your recent work in our community.
Keep up this work! If you need any support with this change open a support ticket!

Staff Member: <@${staff}>
New Rank: ${rank}
Issuer: <@${issuer}>
Reason: ${reason}
Notes: ${notes}`;
}

document.querySelectorAll("#pro-staff,#pro-rank,#pro-issuer,#pro-reason,#pro-notes")
  .forEach(el => el.oninput = updateProPreview);

/* REQUIRED FIELD CHECKS */
function requireFields(fields, statusElement) {
  for (const field of fields) {
    if (!field.value.trim()) {
      statusElement.textContent = `${field.dataset.name} is required.`;
      return false;
    }
  }
  statusElement.textContent = "";
  return true;
}
</script>

</body>
</html>

