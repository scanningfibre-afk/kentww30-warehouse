import { useState, useEffect, useRef, useCallback } from "react";

/* ─── STYLES ────────────────────────────────────────────────────────────── */
const G = {
  bg: "#f4f4f2",
  surface: "#ffffff",
  border: "#e0e0dc",
  amber: "#E8A000",
  amberDark: "#c48600",
  amberLight: "#fff8e6",
  dark: "#1a1a1a",
  muted: "#888",
  green: "#16a34a",
  greenLight: "#f0fdf4",
  red: "#dc2626",
  redLight: "#fef2f2",
  yellow: "#d97706",
  yellowLight: "#fffbeb",
  blue: "#2563eb",
  blueLight: "#eff6ff",
};

/* ─── INITIAL DATA ──────────────────────────────────────────────────────── */
const INIT_PRODUCTS = [
  { code: "P001", barcode: "9300675012345", description: "Heinz Baked Beans 400g", category: "Canned Goods" },
  { code: "P002", barcode: "9300675023456", description: "Coca Cola 330ml Can", category: "Beverages" },
  { code: "P003", barcode: "9300675034567", description: "Kellogg Corn Flakes 500g", category: "Cereals" },
  { code: "P004", barcode: "9300675045678", description: "Nestle Milo 1kg", category: "Beverages" },
  { code: "P005", barcode: "9300675056789", description: "Uncle Tobys Oats 1kg", category: "Cereals" },
];

// Look up product by barcode OR product code
function findProduct(products, input) {
  const q = (input||"").trim().toUpperCase();
  return products.find(p => p.barcode === q || p.code === q || p.code === input?.trim().toUpperCase());
}

// Rack J: J01–J05, each with slots A–E
function buildInitLocations() {
  const locs = {};
  ["J","K","L"].forEach(rack => {
    for (let row = 1; row <= 5; row++) {
      ["A","B","C","D","E"].forEach(slot => {
        const id = `${rack}0${row}${slot}`;
        locs[id] = { id, rack, row, slot, maxProducts: 2, contents: [], allowedProducts: [], customName: "" };
      });
    }
  });
  // Seed some demo stock
  locs["J01A"].contents = [
    { productCode:"P001", description:"Heinz Baked Beans 400g", qty:30, bestBefore:"2026-12-01", storedAt: new Date().toISOString() }
  ];
  locs["J01B"].contents = [
    { productCode:"P002", description:"Coca Cola 330ml Can", qty:20, bestBefore:"2027-03-15", storedAt: new Date().toISOString() },
    { productCode:"P003", description:"Kellogg Corn Flakes 500g", qty:15, bestBefore:"2026-09-30", storedAt: new Date().toISOString() },
  ];
  return locs;
}

const INIT_USERS = {
  admin: { username: "admin", password: "admin123", role: "admin", name: "Admin" },
  picker1: { username: "picker1", password: "pick123", role: "picker", name: "Picker 1" },
  picker2: { username: "picker2", password: "pick456", role: "picker", name: "Picker 2" },
};

/* ─── CONFIG — paste your Google Apps Script URL here after setup ─────── */
const GS_URL = localStorage.getItem("kw_gs_url") || "";

/* ─── LOCAL CACHE (keeps app fast between syncs) ─────────────────────── */
const LS = {
  get: (k, def) => { try { const v = localStorage.getItem(k); return v ? JSON.parse(v) : def; } catch { return def; } },
  set: (k, v) => { try { localStorage.setItem(k, JSON.stringify(v)); } catch {} },
};

/* ─── GOOGLE SHEETS API LAYER ─────────────────────────────────────────── */
const GS = {
  configured: () => !!GS_URL,

  call: async (action, payload = {}) => {
    if (!GS_URL) throw new Error("Google Sheets not configured");
    const res = await fetch(GS_URL, {
      method: "POST",
      headers: { "Content-Type": "text/plain" },
      body: JSON.stringify({ action, ...payload }),
    });
    const text = await res.text();
    try { return JSON.parse(text); }
    catch { throw new Error("Bad response: " + text.slice(0, 200)); }
  },

  // Load all data from Sheets
  loadAll: async () => {
    const r = await GS.call("loadAll");
    return r;
  },

  // Save entire state snapshot
  saveAll: async (data) => {
    return await GS.call("saveAll", { data });
  },

  // Append a log entry
  appendLog: async (entry) => {
    return await GS.call("appendLog", { entry });
  },
};

/* ─── BARCODE SVG ────────────────────────────────────────────────────────── */
function Barcode({ value, width = 200, height = 56 }) {
  if (!value) return null;
  const seed = value.split("").reduce((a, c) => a + c.charCodeAt(0), 0);
  const bars = []; let x = 0;
  for (let i = 0; i < 56; i++) {
    const w = ((seed * (i + 7) * 13) % 3) + 1;
    if (i % 2 === 0) bars.push({ x, w });
    x += w + 1;
  }
  const scale = width / x;
  return (
    <svg width={width} height={height} style={{ display: "block", background: "#fff", borderRadius: 4 }}>
      {bars.map((b, i) => <rect key={i} x={b.x * scale} y={6} width={Math.max(1, b.w * scale)} height={height - 18} fill="#111" />)}
      <text x={width / 2} y={height - 2} textAnchor="middle" fontSize="9" fontFamily="monospace" fill="#444">{value}</text>
    </svg>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   ROOT
══════════════════════════════════════════════════════════════════════════ */
const WAREHOUSES_CONST = ["Dandenong", "Unit 2", "Unit 6"];

function buildInitFloorStock() {
  const fs = {};
  WAREHOUSES_CONST.forEach(wh => { fs[wh] = []; });
  fs["Dandenong"] = [
    { productCode:"P002", description:"Coca Cola 330ml Can",     qty:40, bestBefore:"2027-06-01", storedAt: new Date().toISOString() },
    { productCode:"P002", description:"Coca Cola 330ml Can",     qty:25, bestBefore:"2026-08-15", storedAt: new Date().toISOString() },
    { productCode:"P004", description:"Nestle Milo 1kg",         qty:18, bestBefore:"2027-01-10", storedAt: new Date().toISOString() },
  ];
  fs["Unit 2"] = [
    { productCode:"P001", description:"Heinz Baked Beans 400g", qty:60, bestBefore:"2026-11-30", storedAt: new Date().toISOString() },
  ];
  return fs;
}

export default function App() {
  const [session,    setSession]    = useState(() => LS.get("kw_session", null));
  const [products,   setProducts]   = useState(() => LS.get("kw_products",  INIT_PRODUCTS));
  const [locations,  setLocations]  = useState(() => LS.get("kw_locations", buildInitLocations()));
  const [users,      setUsers]      = useState(() => LS.get("kw_users",     INIT_USERS));
  const [logs,       setLogs]       = useState(() => LS.get("kw_logs",      []));
  const [floorStock, setFloorStock] = useState(() => LS.get("kw_floor",     buildInitFloorStock()));
  const [syncing,    setSyncing]    = useState(false);
  const [syncStatus, setSyncStatus] = useState(null); // "ok" | "err" | null
  const [gsConfigured] = useState(() => !!localStorage.getItem("kw_gs_url"));

  // ── Persist to localStorage (always, as fast local cache) ──
  useEffect(() => LS.set("kw_products",  products),           [products]);
  useEffect(() => LS.set("kw_locations", locations),          [locations]);
  useEffect(() => LS.set("kw_users",     users),              [users]);
  useEffect(() => LS.set("kw_logs",      logs.slice(0, 300)), [logs]);
  useEffect(() => LS.set("kw_floor",     floorStock),         [floorStock]);
  useEffect(() => {
    if (session) LS.set("kw_session", session);
    else localStorage.removeItem("kw_session");
  }, [session]);

  // ── Sync to Google Sheets whenever data changes ──
  const syncTimer = useRef(null);
  const syncToSheets = useCallback((p, l, u, fs) => {
    if (!GS.configured()) return;
    clearTimeout(syncTimer.current);
    syncTimer.current = setTimeout(async () => {
      setSyncing(true);
      try {
        await GS.saveAll({ products: p, locations: l, users: u, floorStock: fs });
        setSyncStatus("ok");
        setTimeout(() => setSyncStatus(null), 2500);
      } catch (e) {
        console.error("Sync error:", e);
        setSyncStatus("err");
        setTimeout(() => setSyncStatus(null), 4000);
      } finally { setSyncing(false); }
    }, 1500); // debounce 1.5s
  }, []);

  useEffect(() => { syncToSheets(products, locations, users, floorStock); },
    [products, locations, users, floorStock, syncToSheets]);

  // ── Load from Google Sheets on startup ──
  useEffect(() => {
    if (!GS.configured()) return;
    (async () => {
      setSyncing(true);
      try {
        const data = await GS.loadAll();
        if (data.products)   setProducts(data.products);
        if (data.locations)  setLocations(data.locations);
        if (data.users)      setUsers(data.users);
        if (data.floorStock) setFloorStock(data.floorStock);
        if (data.logs)       setLogs(data.logs);
        setSyncStatus("ok");
        setTimeout(() => setSyncStatus(null), 2000);
      } catch (e) {
        console.error("Load error:", e);
        setSyncStatus("err");
      } finally { setSyncing(false); }
    })();
  }, []);

  const addLog = useCallback((action, detail, user) => {
    const entry = { ts: new Date().toISOString(), action, detail, user };
    setLogs(l => [entry, ...l]);
    if (GS.configured()) GS.appendLog(entry).catch(console.error);
  }, []);

  const logout = () => setSession(null);

  if (!session) return <LoginScreen users={users} onLogin={setSession} syncStatus={syncStatus} syncing={syncing} gsConfigured={gsConfigured} />;
  if (session.role === "admin") return <AdminApp session={session} products={products} setProducts={setProducts} locations={locations} setLocations={setLocations} users={users} setUsers={setUsers} logs={logs} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} logout={logout} syncing={syncing} syncStatus={syncStatus} />;
  return <PickerApp session={session} products={products} locations={locations} setLocations={setLocations} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} logout={logout} syncing={syncing} syncStatus={syncStatus} />;
}

/* ═══════════════════════════════════════════════════════════════════════════
   LOGIN
══════════════════════════════════════════════════════════════════════════ */
function LoginScreen({ users, onLogin }) {
  const [u, setU] = useState(""); const [p, setP] = useState(""); const [err, setErr] = useState("");
  const submit = () => {
    const user = Object.values(users).find(x => x.username === u.trim() && x.password === p);
    if (user) onLogin(user);
    else { setErr("Invalid username or password"); setTimeout(() => setErr(""), 2500); }
  };
  return (
    <div style={{ minHeight: "100vh", background: G.dark, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", fontFamily: "'Barlow Condensed',sans-serif", padding: 20 }}>
      <style>{css}</style>
      <div style={{ marginBottom: 32, textAlign: "center" }}>
        <div style={{ display: "flex", alignItems: "center", justifyContent: "center", gap: 12, marginBottom: 8 }}>
          <BarcodeIcon />
          <span style={{ fontWeight: 800, fontSize: 28, color: "#fff", letterSpacing: 3 }}>KENTWW30</span>
        </div>
        <div style={{ color: G.amber, fontSize: 13, letterSpacing: 4, fontWeight: 700 }}>WAREHOUSE SYSTEM</div>
      </div>
      <div style={{ background: "#fff", borderRadius: 10, padding: "32px 28px", width: "100%", maxWidth: 380, boxShadow: "0 8px 40px rgba(0,0,0,.4)" }}>
        <div style={{ fontWeight: 800, fontSize: 20, marginBottom: 24, color: G.dark }}>Sign In</div>
        <label className="lbl">USERNAME</label>
        <input className="inp" value={u} onChange={e => setU(e.target.value)} onKeyDown={e => e.key === "Enter" && submit()} autoComplete="username" autoCapitalize="off" />
        <label className="lbl" style={{ marginTop: 14 }}>PASSWORD</label>
        <input className="inp" type="password" value={p} onChange={e => setP(e.target.value)} onKeyDown={e => e.key === "Enter" && submit()} autoComplete="current-password" />
        {err && <div style={{ color: G.red, fontSize: 13, fontWeight: 700, marginTop: 10 }}>{err}</div>}
        <button className="btn-amber" style={{ width: "100%", marginTop: 20, minHeight: 52, fontSize: 17 }} onClick={submit}>SIGN IN</button>

      </div>
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   ADMIN APP
══════════════════════════════════════════════════════════════════════════ */
function AdminApp({ session, products, setProducts, locations, setLocations, users, setUsers, logs, addLog, logout, syncing, syncStatus }) {
  const [tab, setTab] = useState("stock");
  const [toast, setToast] = useState(null);
  const showToast = (msg, type = "ok") => { setToast({ msg, type }); setTimeout(() => setToast(null), 3000); };

  const TABS = [["stock","📦 STOCK VIEW"],["locations","🗂 LOCATIONS"],["products","🏷 PRODUCTS"],["users","👤 USERS"],["logs","📋 LOGS"],["settings","⚙ SETTINGS"]];

  return (
    <div style={{ minHeight: "100vh", background: G.bg, fontFamily: "'Barlow Condensed',sans-serif" }}>
      <style>{css}</style>
      {/* Header */}
      <div style={{ background: G.dark, display: "flex", alignItems: "center", justifyContent: "space-between", padding: "0 20px", height: 54, position: "sticky", top: 0, zIndex: 50 }}>
        <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
          <BarcodeIcon /><span style={{ fontWeight: 800, fontSize: 17, color: "#fff", letterSpacing: 2 }}>KENTWW30</span>
          <span style={{ background: G.amber, color: G.dark, borderRadius: 4, padding: "2px 8px", fontSize: 11, fontWeight: 800, letterSpacing: 1 }}>ADMIN</span>
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: 14 }}>
          {syncing && <div style={{ fontSize: 11, color: G.amber, fontWeight: 700, letterSpacing: 1, animation: "pulse 1.5s infinite" }}>⟳ SYNCING</div>}
          {syncStatus === "ok" && !syncing && <div style={{ fontSize: 11, color: G.green, fontWeight: 700 }}>✓ SAVED</div>}
          {syncStatus === "err" && <div style={{ fontSize: 11, color: G.red, fontWeight: 700 }}>⚠ SYNC ERROR</div>}
          <span style={{ color: G.muted, fontSize: 13 }}>{session.name}</span>
          <button className="btn-ghost-sm" onClick={logout}>LOGOUT</button>
        </div>
      </div>
      {/* Tab bar */}
      <div style={{ background: "#222", display: "flex", overflowX: "auto", scrollbarWidth: "none" }}>
        {TABS.map(([id, label]) => (
          <button key={id} onClick={() => setTab(id)} style={{ background: "none", border: "none", borderBottom: `3px solid ${tab === id ? G.amber : "transparent"}`, color: tab === id ? G.amber : "#888", fontFamily: "'Barlow Condensed',sans-serif", fontWeight: 700, fontSize: 13, padding: "0 18px", height: 46, cursor: "pointer", whiteSpace: "nowrap", letterSpacing: 1 }}>{label}</button>
        ))}
      </div>
      {/* Body */}
      <div style={{ padding: 20, maxWidth: 1100, margin: "0 auto" }}>
        {tab === "stock"     && <AdminStock     locations={locations} products={products} />}
        {tab === "locations" && <AdminLocations locations={locations} setLocations={setLocations} products={products} addLog={addLog} session={session} showToast={showToast} />}
        {tab === "products"  && <AdminProducts  products={products}   setProducts={setProducts} addLog={addLog} session={session} showToast={showToast} />}
        {tab === "users"     && <AdminUsers     users={users}         setUsers={setUsers} session={session} showToast={showToast} />}
        {tab === "logs"      && <AdminLogs      logs={logs} />}
        {tab === "settings"  && <AdminSettings  locations={locations} setLocations={setLocations} showToast={showToast} />}
      </div>
      {toast && <Toast toast={toast} />}
    </div>
  );
}

/* ── Admin: Stock View ──────────────────────────────────────────────────── */
function AdminStock({ locations, products }) {
  const [search, setSearch] = useState("");
  const [rack, setRack] = useState("All");
  const racks = ["All", ...new Set(Object.values(locations).map(l => l.rack))];

  const rows = [];
  Object.values(locations).forEach(loc => {
    loc.contents.forEach(c => {
      rows.push({ loc: loc.id, rack: loc.rack, row: loc.row, slot: loc.slot, ...c });
    });
  });

  const filtered = rows.filter(r => {
    const ms = !search || r.productCode.toLowerCase().includes(search.toLowerCase()) || r.description.toLowerCase().includes(search.toLowerCase()) || r.loc.toLowerCase().includes(search.toLowerCase());
    const mr = rack === "All" || r.rack === rack;
    return ms && mr;
  });

  const expWarn = (bb) => {
    if (!bb) return null;
    const d = new Date(bb), now = new Date();
    if (d < now) return "expired";
    if ((d - now) < 90 * 86400000) return "soon";
    return null;
  };

  return (
    <div>
      <div style={{ fontWeight: 800, fontSize: 22, marginBottom: 16 }}>📦 STOCK VIEW</div>
      <div style={{ display: "flex", gap: 10, marginBottom: 16, flexWrap: "wrap" }}>
        <input className="inp" style={{ maxWidth: 280 }} placeholder="Search product, code, location…" value={search} onChange={e => setSearch(e.target.value)} />
        <select className="sel" style={{ width: 120 }} value={rack} onChange={e => setRack(e.target.value)}>
          {racks.map(r => <option key={r}>{r}</option>)}
        </select>
        <div style={{ marginLeft: "auto", fontSize: 14, color: G.muted, alignSelf: "center" }}>{filtered.length} entries</div>
      </div>
      <div style={{ background: G.surface, borderRadius: 8, overflow: "hidden", boxShadow: "0 1px 4px rgba(0,0,0,.07)" }}>
        <table style={{ width: "100%", borderCollapse: "collapse", fontSize: 14 }}>
          <thead>
            <tr style={{ background: G.dark, color: "#fff" }}>
              {["LOCATION","PRODUCT CODE","DESCRIPTION","BEST BEFORE","QTY","STORED"].map(h => (
                <th key={h} style={{ padding: "11px 14px", textAlign: "left", fontSize: 11, fontWeight: 700, letterSpacing: 1 }}>{h}</th>
              ))}
            </tr>
          </thead>
          <tbody>
            {filtered.map((r, i) => {
              const w = expWarn(r.bestBefore);
              return (
                <tr key={i} style={{ borderBottom: `1px solid ${G.border}`, background: i % 2 === 0 ? "#fff" : "#fafaf8" }}>
                  <td style={{ padding: "11px 14px", fontWeight: 800, color: G.amber, fontFamily: "monospace" }}>{r.loc}</td>
                  <td style={{ padding: "11px 14px", fontFamily: "monospace", fontWeight: 700 }}>{r.productCode}</td>
                  <td style={{ padding: "11px 14px" }}>{r.description}</td>
                  <td style={{ padding: "11px 14px" }}>
                    <span style={{ color: w === "expired" ? G.red : w === "soon" ? G.yellow : G.dark, fontWeight: w ? 700 : 400 }}>
                      {r.bestBefore || "—"}{w === "expired" ? " ⚠" : w === "soon" ? " ⏰" : ""}
                    </span>
                  </td>
                  <td style={{ padding: "11px 14px", fontWeight: 800, fontSize: 16 }}>{r.qty}</td>
                  <td style={{ padding: "11px 14px", color: G.muted, fontSize: 12 }}>{r.storedAt ? new Date(r.storedAt).toLocaleDateString() : "—"}</td>
                </tr>
              );
            })}
            {filtered.length === 0 && <tr><td colSpan={6} style={{ padding: 32, textAlign: "center", color: G.muted }}>No stock entries found</td></tr>}
          </tbody>
        </table>
      </div>
    </div>
  );
}

/* ── Admin: Locations ───────────────────────────────────────────────────── */
function AdminLocations({ locations, setLocations, products, addLog, session, showToast }) {
  const [rack, setRack] = useState("All");
  const [editLoc, setEditLoc] = useState(null);
  const [editingName, setEditingName] = useState(null);
  const [editingAllowed, setEditingAllowed] = useState(null);
  const [addingLoc, setAddingLoc] = useState(false);
  const [newLocForm, setNewLocForm] = useState({ id: "", rack: "", maxProducts: 2 });
  const racks = ["All", ...new Set(Object.values(locations).map(l => l.rack))];
  const filtered = Object.values(locations).filter(l => rack === "All" || l.rack === rack).sort((a,b) => a.id.localeCompare(b.id));

  const removeContent = (locId, idx) => {
    setLocations(prev => ({ ...prev, [locId]: { ...prev[locId], contents: prev[locId].contents.filter((_,i) => i !== idx) } }));
    addLog("ADMIN_REMOVE", `Removed item from ${locId}`, session.username);
    showToast("Item removed");
  };
  const editContent = (locId, idx, field, val) => {
    setLocations(prev => ({ ...prev, [locId]: { ...prev[locId], contents: prev[locId].contents.map((c,i) => i===idx?{...c,[field]:val}:c) } }));
  };
  const removeLocation = (locId) => {
    if (!window.confirm(`Delete location ${locId}? This cannot be undone.`)) return;
    setLocations(prev => { const n={...prev}; delete n[locId]; return n; });
    showToast(`Location ${locId} deleted`);
    addLog("ADMIN_DEL_LOC", `Deleted location ${locId}`, session.username);
  };
  const saveName = (locId, name) => {
    setLocations(prev => ({ ...prev, [locId]: { ...prev[locId], customName: name } }));
    setEditingName(null); showToast("Name saved");
  };
  const saveAllowed = (locId, allowed) => {
    setLocations(prev => ({ ...prev, [locId]: { ...prev[locId], allowedProducts: allowed } }));
    setEditingAllowed(null); showToast("Allowed products saved");
  };
  const addLocation = () => {
    const id = newLocForm.id.trim().toUpperCase();
    if (!id) { showToast("Enter location ID", "err"); return; }
    if (locations[id]) { showToast("Location already exists", "err"); return; }
    setLocations(prev => ({ ...prev, [id]: { id, rack: newLocForm.rack || id[0], row: 0, slot: "", maxProducts: parseInt(newLocForm.maxProducts)||2, contents: [], allowedProducts: [], customName: "" } }));
    setAddingLoc(false); setNewLocForm({ id:"", rack:"", maxProducts:2 });
    showToast(`Location ${id} added`);
    addLog("ADMIN_ADD_LOC", `Added location ${id}`, session.username);
  };

  return (
    <div>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:16 }}>
        <div style={{ fontWeight:800, fontSize:22 }}>🗂 LOCATIONS</div>
        <button className="btn-amber" onClick={() => setAddingLoc(a=>!a)}>+ ADD LOCATION</button>
      </div>

      {addingLoc && (
        <div style={{ background:G.amberLight, border:`1px solid ${G.amber}`, borderRadius:8, padding:18, marginBottom:16 }}>
          <div style={{ fontWeight:700, fontSize:15, marginBottom:12 }}>New Location</div>
          <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr 120px", gap:12 }}>
            <div><label className="lbl">LOCATION ID *</label><input className="inp" value={newLocForm.id} onChange={e=>setNewLocForm(f=>({...f,id:e.target.value.toUpperCase()}))} placeholder="J06A"/></div>
            <div><label className="lbl">RACK</label><input className="inp" value={newLocForm.rack} onChange={e=>setNewLocForm(f=>({...f,rack:e.target.value.toUpperCase()}))} placeholder="J"/></div>
            <div><label className="lbl">MAX PRODUCTS</label>
              <select className="sel" value={newLocForm.maxProducts} onChange={e=>setNewLocForm(f=>({...f,maxProducts:e.target.value}))}>
                {[1,2,3,4,5].map(n=><option key={n}>{n}</option>)}
              </select>
            </div>
          </div>
          <div style={{ display:"flex", gap:10, marginTop:14 }}>
            <button className="btn-amber" onClick={addLocation}>ADD</button>
            <button className="btn-ghost-sm" onClick={()=>setAddingLoc(false)}>CANCEL</button>
          </div>
        </div>
      )}

      <div style={{ display:"flex", gap:8, marginBottom:16, flexWrap:"wrap" }}>
        {racks.map(r => (
          <button key={r} onClick={()=>setRack(r)} className={rack===r?"btn-amber-sm":"btn-ghost-sm"}>{r==="All"?"ALL RACKS":`RACK ${r}`}</button>
        ))}
      </div>

      <div style={{ display:"grid", gap:10 }}>
        {filtered.map(loc => {
          const free = loc.maxProducts - loc.contents.length;
          const isEditingAllowed = editingAllowed === loc.id;
          const isEditingName = editingName === loc.id;
          return (
            <div key={loc.id} style={{ background:G.surface, borderRadius:8, border:`1px solid ${G.border}`, overflow:"hidden" }}>
              {/* Location header */}
              <div style={{ display:"flex", alignItems:"center", gap:12, padding:"12px 16px", background:"#fafaf8", borderBottom:`1px solid ${G.border}`, flexWrap:"wrap" }}>
                <div style={{ fontWeight:800, fontSize:18, color:G.amber, fontFamily:"monospace", minWidth:60 }}>{loc.id}</div>
                {/* Custom name */}
                {isEditingName ? (
                  <NameEditor locId={loc.id} current={loc.customName||""} onSave={saveName} onCancel={()=>setEditingName(null)} />
                ) : (
                  <div style={{ display:"flex", alignItems:"center", gap:6 }}>
                    <span style={{ fontSize:13, color: loc.customName ? G.dark : G.muted, fontStyle: loc.customName ? "normal":"italic" }}>{loc.customName || "No label"}</span>
                    <button className="btn-ghost-sm" style={{ padding:"2px 8px", fontSize:11 }} onClick={()=>setEditingName(loc.id)}>✏</button>
                  </div>
                )}
                <div style={{ fontSize:13, color:G.muted }}>Max: <b style={{color:G.dark}}>{loc.maxProducts}</b></div>
                <div style={{ fontSize:13 }}><span style={{ color:free>0?G.green:G.red, fontWeight:700 }}>{free} free</span></div>
                {/* Allowed products badge */}
                {loc.allowedProducts?.length > 0 && (
                  <div style={{ background:"#eff6ff", border:"1px solid #93c5fd", borderRadius:4, padding:"2px 8px", fontSize:11, color:G.blue, fontWeight:700 }}>
                    {loc.allowedProducts.length} RECOMMENDED
                  </div>
                )}
                <div style={{ display:"flex", gap:4, marginLeft:"auto" }}>
                  {Array.from({length:loc.maxProducts}).map((_,i) => (
                    <div key={i} style={{ width:18, height:18, borderRadius:4, background:i<loc.contents.length?G.amber:G.border, border:`1px solid ${i<loc.contents.length?G.amberDark:"#ccc"}` }}/>
                  ))}
                </div>
                <button className="btn-ghost-sm" style={{ padding:"4px 10px", fontSize:11 }} onClick={()=>setEditingAllowed(isEditingAllowed?null:loc.id)}>
                  {isEditingAllowed ? "CLOSE" : "SET PRODUCTS"}
                </button>
                <button className="btn-red-sm" style={{ padding:"4px 10px", fontSize:11 }} onClick={()=>removeLocation(loc.id)}>DEL</button>
              </div>

              {/* Allowed products editor */}
              {isEditingAllowed && (
                <AllowedProductsEditor loc={loc} products={products} onSave={saveAllowed} onCancel={()=>setEditingAllowed(null)} />
              )}

              {/* Contents */}
              {loc.contents.length === 0 && !isEditingAllowed && (
                <div style={{ padding:"12px 16px", color:G.muted, fontSize:13, fontStyle:"italic" }}>Empty location</div>
              )}
              {loc.contents.map((c, idx) => (
                <div key={idx} style={{ padding:"12px 16px", borderBottom:`1px solid ${G.border}`, display:"flex", gap:10, alignItems:"center", flexWrap:"wrap" }}>
                  <div style={{ flex:1, minWidth:200 }}>
                    <div style={{ fontWeight:700, fontSize:15 }}>{c.description}</div>
                    <div style={{ fontSize:12, color:G.muted, fontFamily:"monospace" }}>{c.productCode}</div>
                  </div>
                  {editLoc === `${loc.id}-${idx}` ? (
                    <div style={{ display:"flex", gap:8, alignItems:"center", flexWrap:"wrap" }}>
                      <div><label className="lbl">BEST BEFORE</label><input className="inp" type="date" value={c.bestBefore} onChange={e=>editContent(loc.id,idx,"bestBefore",e.target.value)} style={{ minHeight:38, padding:"6px 10px" }}/></div>
                      <div><label className="lbl">QTY</label><input className="inp" type="number" value={c.qty} onChange={e=>editContent(loc.id,idx,"qty",parseInt(e.target.value)||0)} style={{ width:90, minHeight:38, padding:"6px 10px" }}/></div>
                      <button className="btn-green-sm" style={{ marginTop:18 }} onClick={()=>{ setEditLoc(null); showToast("Saved"); addLog("ADMIN_EDIT",`Edited ${c.productCode} at ${loc.id}`,session.username); }}>SAVE</button>
                    </div>
                  ) : (
                    <>
                      <div style={{ textAlign:"center", minWidth:90 }}>
                        <div style={{ fontSize:11, color:G.muted, letterSpacing:1 }}>BEST BEFORE</div>
                        <div style={{ fontWeight:700, color:c.bestBefore&&new Date(c.bestBefore)<new Date()?G.red:G.dark }}>{c.bestBefore||"—"}</div>
                      </div>
                      <div style={{ textAlign:"center", minWidth:60 }}>
                        <div style={{ fontSize:11, color:G.muted, letterSpacing:1 }}>QTY</div>
                        <div style={{ fontWeight:800, fontSize:20 }}>{c.qty}</div>
                      </div>
                      <button className="btn-ghost-sm" onClick={()=>setEditLoc(`${loc.id}-${idx}`)}>EDIT</button>
                      <button className="btn-red-sm" onClick={()=>{ if(window.confirm(`Remove ${c.description} from ${loc.id}?`)) removeContent(loc.id,idx); }}>REMOVE</button>
                    </>
                  )}
                </div>
              ))}
            </div>
          );
        })}
      </div>
    </div>
  );
}

/* Inline name editor */
function NameEditor({ locId, current, onSave, onCancel }) {
  const [val, setVal] = useState(current);
  return (
    <div style={{ display:"flex", gap:6, alignItems:"center" }}>
      <input className="inp" value={val} onChange={e=>setVal(e.target.value)} placeholder="e.g. Beverages Row" style={{ minHeight:34, padding:"4px 10px", fontSize:13, width:180 }}/>
      <button className="btn-green-sm" style={{ padding:"4px 10px" }} onClick={()=>onSave(locId, val)}>SAVE</button>
      <button className="btn-ghost-sm" style={{ padding:"4px 10px" }} onClick={onCancel}>✕</button>
    </div>
  );
}

/* Allowed products editor — up to 5 */
function AllowedProductsEditor({ loc, products, onSave, onCancel }) {
  const [selected, setSelected] = useState(loc.allowedProducts || []);
  const [search, setSearch] = useState("");
  const filtered = products.filter(p =>
    !search || p.description.toLowerCase().includes(search.toLowerCase()) || p.code.toLowerCase().includes(search.toLowerCase())
  );
  const toggle = (code) => {
    if (selected.includes(code)) setSelected(s => s.filter(c => c !== code));
    else if (selected.length < 5) setSelected(s => [...s, code]);
  };
  return (
    <div style={{ background:G.blueLight, borderBottom:`1px solid #bfdbfe`, padding:"14px 16px" }}>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:10 }}>
        <div>
          <div style={{ fontWeight:700, fontSize:14, color:G.blue }}>RECOMMENDED PRODUCTS for {loc.id}</div>
          <div style={{ fontSize:12, color:G.muted, marginTop:2 }}>Select up to 5 — picker will see a recommendation (not a block)</div>
        </div>
        <span style={{ background:G.blue, color:"#fff", borderRadius:4, padding:"2px 8px", fontSize:12, fontWeight:700 }}>{selected.length}/5</span>
      </div>
      <input className="inp" value={search} onChange={e=>setSearch(e.target.value)} placeholder="Search products…" style={{ marginBottom:10, minHeight:36, padding:"6px 10px", fontSize:13 }}/>
      <div style={{ maxHeight:200, overflowY:"auto", display:"grid", gap:6, marginBottom:12 }}>
        {filtered.map(p => {
          const on = selected.includes(p.code);
          const disabled = !on && selected.length >= 5;
          return (
            <div key={p.code} onClick={()=>!disabled&&toggle(p.code)}
              style={{ display:"flex", alignItems:"center", gap:10, padding:"8px 12px", borderRadius:6, border:`2px solid ${on?G.blue:G.border}`, background:on?"#dbeafe":"#fff", cursor:disabled?"not-allowed":"pointer", opacity:disabled?.5:1 }}>
              <div style={{ width:18, height:18, borderRadius:4, border:`2px solid ${on?G.blue:"#ccc"}`, background:on?G.blue:"#fff", display:"flex", alignItems:"center", justifyContent:"center", flexShrink:0 }}>
                {on && <span style={{ color:"#fff", fontSize:12, fontWeight:800 }}>✓</span>}
              </div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:600, fontSize:14 }}>{p.description}</div>
                <div style={{ fontSize:11, color:G.muted, fontFamily:"monospace" }}>{p.code} {p.barcode ? `· ${p.barcode}` : ""}</div>
              </div>
            </div>
          );
        })}
      </div>
      {selected.length > 0 && (
        <div style={{ marginBottom:10, fontSize:12, color:G.blue, fontWeight:600 }}>
          Selected: {selected.map(c => products.find(p=>p.code===c)?.description || c).join(", ")}
        </div>
      )}
      <div style={{ display:"flex", gap:8 }}>
        <button className="btn-amber" onClick={()=>onSave(loc.id, selected)}>SAVE</button>
        <button className="btn-ghost-sm" onClick={()=>onSave(loc.id, [])}>CLEAR ALL</button>
        <button className="btn-ghost-sm" onClick={onCancel}>CANCEL</button>
      </div>
    </div>
  );
}
/* ── Admin: Products ────────────────────────────────────────────────────── */
function AdminProducts({ products, setProducts, addLog, session, showToast }) {
  const [editing, setEditing] = useState(null);
  const [form, setForm] = useState({ code:"", barcode:"", description:"", category:"" });
  const [adding, setAdding] = useState(false);
  const [search, setSearch] = useState("");

  const startEdit = (p) => { setEditing(p.code); setForm({ code:p.code, barcode:p.barcode||"", description:p.description, category:p.category||"" }); setAdding(false); };
  const startAdd  = () => { setAdding(true); setEditing(null); setForm({ code:"", barcode:"", description:"", category:"" }); };
  const save = () => {
    if (!form.code.trim() || !form.description.trim()) { showToast("Code and description required","err"); return; }
    const code = form.code.trim().toUpperCase();
    if (adding) {
      if (products.find(p => p.code === code)) { showToast("Code already exists","err"); return; }
      setProducts(prev => [...prev, { code, barcode: form.barcode||"", description: form.description, category: form.category||"" }]);
      addLog("PRODUCT_ADD", form.description, session.username);
    } else {
      setProducts(prev => prev.map(x => x.code === editing ? { code, barcode: form.barcode||"", description: form.description, category: form.category||"" } : x));
      addLog("PRODUCT_EDIT", form.description, session.username);
    }
    showToast(adding ? "Product added" : "Product updated");
    setEditing(null); setAdding(false);
  };

  const filtered = products.filter(p =>
    !search || p.code.toLowerCase().includes(search.toLowerCase()) ||
    (p.barcode||"").includes(search) ||
    p.description.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:16 }}>
        <div style={{ fontWeight:800, fontSize:22 }}>🏷 PRODUCTS</div>
        <button className="btn-amber" onClick={startAdd}>+ ADD PRODUCT</button>
      </div>
      <input className="inp" style={{ marginBottom:14, maxWidth:340 }} placeholder="Search by code, barcode or name…"
        value={search} onChange={e=>setSearch(e.target.value)} />
      {(adding || editing) && (
        <div style={{ background:G.amberLight, border:`1px solid ${G.amber}`, borderRadius:8, padding:18, marginBottom:16 }}>
          <div style={{ fontWeight:700, marginBottom:12, fontSize:15 }}>{adding ? "New Product" : `Editing ${editing}`}</div>
          <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12 }}>
            <div><label className="lbl">PRODUCT CODE *</label><input className="inp" value={form.code} onChange={e=>setForm(f=>({...f,code:e.target.value.toUpperCase()}))} placeholder="P006"/></div>
            <div><label className="lbl">BARCODE (SCANNER)</label><input className="inp" value={form.barcode} onChange={e=>setForm(f=>({...f,barcode:e.target.value}))} placeholder="9300675012345"/></div>
            <div style={{ gridColumn:"1/-1" }}><label className="lbl">DESCRIPTION *</label><input className="inp" value={form.description} onChange={e=>setForm(f=>({...f,description:e.target.value}))} placeholder="Product name"/></div>
            <div style={{ gridColumn:"1/-1" }}><label className="lbl">CATEGORY</label><input className="inp" value={form.category} onChange={e=>setForm(f=>({...f,category:e.target.value}))} placeholder="Category"/></div>
          </div>
          <div style={{ fontSize:12, color:G.muted, marginTop:8 }}>Barcode = scanner input · Product Code = manual search</div>
          <div style={{ display:"flex", gap:10, marginTop:14 }}>
            <button className="btn-amber" onClick={save}>SAVE</button>
            <button className="btn-ghost-sm" onClick={()=>{ setEditing(null); setAdding(false); }}>CANCEL</button>
          </div>
        </div>
      )}
      <div style={{ background:G.surface, borderRadius:8, overflow:"hidden", boxShadow:"0 1px 4px rgba(0,0,0,.07)" }}>
        <table style={{ width:"100%", borderCollapse:"collapse", fontSize:14 }}>
          <thead>
            <tr style={{ background:G.dark, color:"#fff" }}>
              {["PRODUCT CODE","BARCODE","DESCRIPTION","CATEGORY",""].map(h =>
                <th key={h} style={{ padding:"11px 14px", textAlign:"left", fontSize:11, fontWeight:700, letterSpacing:1 }}>{h}</th>
              )}
            </tr>
          </thead>
          <tbody>
            {filtered.map((p, i) => (
              <tr key={p.code} style={{ borderBottom:`1px solid ${G.border}`, background:i%2===0?"#fff":"#fafaf8" }}>
                <td style={{ padding:"12px 14px", fontFamily:"monospace", fontWeight:800, color:G.amber }}>{p.code}</td>
                <td style={{ padding:"12px 14px", fontFamily:"monospace", fontSize:13, color:G.muted }}>{p.barcode||"—"}</td>
                <td style={{ padding:"12px 14px", fontWeight:600 }}>{p.description}</td>
                <td style={{ padding:"12px 14px", color:G.muted }}>{p.category||"—"}</td>
                <td style={{ padding:"12px 14px" }}><button className="btn-ghost-sm" onClick={()=>startEdit(p)}>EDIT</button></td>
              </tr>
            ))}
            {filtered.length===0 && <tr><td colSpan={5} style={{ padding:24, textAlign:"center", color:G.muted }}>No products found</td></tr>}
          </tbody>
        </table>
      </div>
    </div>
  );
}
/* ── Admin: Users ───────────────────────────────────────────────────────── */
function AdminUsers({ users, setUsers, session, showToast }) {
  const [adding, setAdding] = useState(false);
  const [form, setForm] = useState({ username: "", password: "", name: "", role: "picker" });

  const addUser = () => {
    if (!form.username.trim() || !form.password.trim() || !form.name.trim()) { showToast("All fields required", "err"); return; }
    if (users[form.username]) { showToast("Username taken", "err"); return; }
    setUsers(u => ({ ...u, [form.username]: { ...form } }));
    showToast("User created");
    setAdding(false); setForm({ username: "", password: "", name: "", role: "picker" });
  };

  const removeUser = (uname) => {
    if (uname === session.username) { showToast("Cannot delete yourself", "err"); return; }
    if (!window.confirm(`Delete user ${uname}?`)) return;
    setUsers(u => { const n = { ...u }; delete n[uname]; return n; });
    showToast("User removed");
  };

  return (
    <div>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 16 }}>
        <div style={{ fontWeight: 800, fontSize: 22 }}>👤 USERS</div>
        <button className="btn-amber" onClick={() => setAdding(a => !a)}>+ ADD USER</button>
      </div>
      {adding && (
        <div style={{ background: G.amberLight, border: `1px solid ${G.amber}`, borderRadius: 8, padding: 18, marginBottom: 16 }}>
          <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr 1fr 120px", gap: 12 }}>
            <div><label className="lbl">NAME</label><input className="inp" value={form.name} onChange={e => setForm(f => ({ ...f, name: e.target.value }))} /></div>
            <div><label className="lbl">USERNAME</label><input className="inp" value={form.username} onChange={e => setForm(f => ({ ...f, username: e.target.value.toLowerCase() }))} /></div>
            <div><label className="lbl">PASSWORD</label><input className="inp" value={form.password} onChange={e => setForm(f => ({ ...f, password: e.target.value }))} /></div>
            <div><label className="lbl">ROLE</label>
              <select className="sel" value={form.role} onChange={e => setForm(f => ({ ...f, role: e.target.value }))}>
                <option value="picker">Picker</option><option value="admin">Admin</option>
              </select>
            </div>
          </div>
          <div style={{ display: "flex", gap: 10, marginTop: 14 }}>
            <button className="btn-amber" onClick={addUser}>CREATE</button>
            <button className="btn-ghost-sm" onClick={() => setAdding(false)}>CANCEL</button>
          </div>
        </div>
      )}
      <div style={{ display: "grid", gap: 10 }}>
        {Object.values(users).map(u => (
          <div key={u.username} style={{ background: G.surface, borderRadius: 8, border: `1px solid ${G.border}`, padding: "14px 18px", display: "flex", alignItems: "center", gap: 14 }}>
            <div style={{ width: 40, height: 40, borderRadius: "50%", background: u.role === "admin" ? G.amber : G.blue, display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 800, fontSize: 16, color: "#fff" }}>{u.name[0]}</div>
            <div style={{ flex: 1 }}>
              <div style={{ fontWeight: 700, fontSize: 16 }}>{u.name}</div>
              <div style={{ fontSize: 13, color: G.muted }}>@{u.username}</div>
            </div>
            <span style={{ background: u.role === "admin" ? G.amberLight : G.blueLight, color: u.role === "admin" ? G.amberDark : G.blue, border: `1px solid ${u.role === "admin" ? G.amber : "#93c5fd"}`, borderRadius: 4, padding: "3px 10px", fontSize: 12, fontWeight: 700 }}>{u.role.toUpperCase()}</span>
            {u.username !== session.username && <button className="btn-red-sm" onClick={() => removeUser(u.username)}>REMOVE</button>}
          </div>
        ))}
      </div>
    </div>
  );
}

/* ── Admin: Settings ────────────────────────────────────────────────────── */
function AdminSettings({ locations, setLocations, showToast }) {
  const [maxProd, setMaxProd] = useState(2);
  const [rack, setRack] = useState("All");
  const racks = ["All", ...new Set(Object.values(locations).map(l => l.rack))];

  const applyToAll = () => {
    if (!window.confirm(`Set max products to ${maxProd} for ${rack === "All" ? "ALL locations" : `Rack ${rack}`}?`)) return;
    setLocations(prev => {
      const next = { ...prev };
      Object.keys(next).forEach(id => {
        if (rack === "All" || next[id].rack === rack) next[id] = { ...next[id], maxProducts: parseInt(maxProd) };
      });
      return next;
    });
    showToast(`Max products set to ${maxProd} for ${rack === "All" ? "all locations" : `Rack ${rack}`}`);
  };

  return (
    <div style={{ maxWidth: 540 }}>
      <div style={{ fontWeight: 800, fontSize: 22, marginBottom: 20 }}>⚙ SETTINGS</div>
      <div style={{ background: G.surface, borderRadius: 8, border: `1px solid ${G.border}`, padding: 24, marginBottom: 16 }}>
        <div style={{ fontWeight: 700, fontSize: 16, marginBottom: 6 }}>Location Capacity</div>
        <div style={{ fontSize: 14, color: G.muted, marginBottom: 18 }}>Set how many different products can be stored in each bin location. Changing this affects what pickers can store going forward.</div>
        <label className="lbl">APPLY TO RACK</label>
        <select className="sel" style={{ width: 160, marginBottom: 14 }} value={rack} onChange={e => setRack(e.target.value)}>
          {racks.map(r => <option key={r}>{r}</option>)}
        </select>
        <label className="lbl">MAX PRODUCTS PER LOCATION</label>
        <div style={{ display: "flex", gap: 10, alignItems: "center", marginTop: 4 }}>
          {[1, 2, 3, 4].map(n => (
            <button key={n} onClick={() => setMaxProd(n)} style={{ width: 52, height: 52, borderRadius: 8, border: `2px solid ${maxProd === n ? G.amber : G.border}`, background: maxProd === n ? G.amberLight : "#fff", fontWeight: 800, fontSize: 20, cursor: "pointer", color: maxProd === n ? G.amberDark : G.muted, fontFamily: "'Barlow Condensed',sans-serif" }}>{n}</button>
          ))}
        </div>
        <button className="btn-amber" style={{ marginTop: 20, minHeight: 48, fontSize: 15 }} onClick={applyToAll}>APPLY SETTING</button>
      </div>
    </div>
  );
}

/* ── Admin: Logs ────────────────────────────────────────────────────────── */
function AdminLogs({ logs }) {
  const colMap = { STORE: [G.greenLight, G.green], ADMIN_EDIT: [G.blueLight, G.blue], ADMIN_REMOVE: [G.redLight, G.red], PRODUCT_ADD: [G.amberLight, G.amberDark], PRODUCT_EDIT: [G.amberLight, G.amberDark], COUNT: ["#f0f9ff", "#0284c7"] };
  return (
    <div>
      <div style={{ fontWeight: 800, fontSize: 22, marginBottom: 16 }}>📋 ACTIVITY LOGS</div>
      <div style={{ background: G.surface, borderRadius: 8, overflow: "hidden", boxShadow: "0 1px 4px rgba(0,0,0,.07)" }}>
        {logs.length === 0 && <div style={{ padding: 32, textAlign: "center", color: G.muted }}>No activity yet</div>}
        {logs.map((l, i) => {
          const [bg, fg] = colMap[l.action] || ["#f5f5f3", G.muted];
          return (
            <div key={i} style={{ display: "flex", gap: 12, padding: "11px 16px", borderBottom: `1px solid ${G.border}`, alignItems: "center" }}>
              <div style={{ fontSize: 11, color: G.muted, minWidth: 85, fontFamily: "monospace" }}>{new Date(l.ts).toLocaleTimeString()}</div>
              <span style={{ background: bg, color: fg, border: `1px solid ${fg}30`, borderRadius: 4, padding: "2px 8px", fontSize: 11, fontWeight: 700, minWidth: 100, textAlign: "center" }}>{l.action}</span>
              <div style={{ flex: 1, fontSize: 14 }}>{l.detail}</div>
              <div style={{ fontSize: 12, color: G.muted }}>@{l.user}</div>
              <div style={{ fontSize: 11, color: "#bbb" }}>{new Date(l.ts).toLocaleDateString()}</div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   PICKER APP — root
══════════════════════════════════════════════════════════════════════════ */
const WAREHOUSES = WAREHOUSES_CONST;

function PickerApp({ session, products, locations, setLocations, floorStock, setFloorStock, addLog, logout, syncing, syncStatus }) {
  const [mode, setMode] = useState(null); // null | "add" | "remove_aisle" | "remove_floor" | "move"
  const [warehouse, setWarehouse] = useState(null);
  const [toast, setToast] = useState(null);
  const showToast = (msg, type = "ok") => { setToast({ msg, type }); setTimeout(() => setToast(null), 3500); };
  const goHome = () => { setMode(null); setWarehouse(null); };

  return (
    <div style={{ minHeight: "100vh", background: G.dark, fontFamily: "'Barlow Condensed',sans-serif" }}>
      <style>{css}</style>
      {/* Header */}
      <div style={{ background: "#111", borderBottom: `3px solid ${G.amber}`, display: "flex", alignItems: "center", justifyContent: "space-between", padding: "0 16px", height: 54, position: "sticky", top: 0, zIndex: 50 }}>
        <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
          {mode && <button onClick={goHome} style={{ background: "none", border: "none", color: G.amber, fontSize: 24, cursor: "pointer", lineHeight: 1, padding: "0 4px" }}>‹</button>}
          <BarcodeIcon />
          <span style={{ fontWeight: 800, fontSize: 17, color: "#fff", letterSpacing: 2 }}>KENTWW30</span>
          <span style={{ background: "#333", color: "#aaa", borderRadius: 4, padding: "2px 8px", fontSize: 11, fontWeight: 700 }}>PICKER</span>
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
          {syncing && <div style={{ fontSize: 11, color: G.amber, fontWeight: 700, animation: "pulse 1.5s infinite" }}>⟳</div>}
          {syncStatus === "ok" && !syncing && <div style={{ fontSize: 11, color: G.green, fontWeight: 700 }}>✓</div>}
          {syncStatus === "err" && <div style={{ fontSize: 11, color: G.red, fontWeight: 700 }}>⚠</div>}
          <span style={{ color: G.amber, fontSize: 13, fontWeight: 700 }}>{session.name}</span>
          <button className="btn-ghost-sm" onClick={logout}>LOGOUT</button>
        </div>
      </div>
      {/* Breadcrumb */}
      {mode && (
        <div style={{ background: "#1a1a1a", borderBottom: "1px solid #2a2a2a", padding: "8px 16px", fontSize: 12, color: "#555", display: "flex", gap: 6 }}>
          <span style={{ color: G.amber, cursor: "pointer", fontWeight: 700 }} onClick={goHome}>HOME</span>
          <span>›</span>
          <span style={{ color: "#aaa", fontWeight: 700 }}>
            {mode === "add" && "ADD STOCK"}
            {mode === "remove_aisle" && "REMOVE — AISLE"}
            {mode === "remove_floor" && `REMOVE — FLOOR · ${warehouse?.toUpperCase()}`}
            {mode === "move" && "MOVE STOCK"}
          </span>
        </div>
      )}
      <div style={{ padding: 16, maxWidth: 520, margin: "0 auto" }}>
        {!mode && <PickerHome session={session} onAddStock={() => setMode("add")} onRemoveAisle={() => setMode("remove_aisle")} onRemoveFloor={(wh) => { setWarehouse(wh); setMode("remove_floor"); }} onMoveStock={() => setMode("move")} />}
        {mode === "add"           && <AddStockFlow    session={session} products={products} locations={locations} setLocations={setLocations} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} showToast={showToast} onDone={goHome} />}
        {mode === "remove_aisle"  && <RemoveAisleFlow session={session} locations={locations} setLocations={setLocations} addLog={addLog} showToast={showToast} onDone={goHome} />}
        {mode === "remove_floor"  && <RemoveFloorFlow session={session} warehouse={warehouse} products={products} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} showToast={showToast} onDone={goHome} />}
        {mode === "move"           && <MoveStockFlow   session={session} products={products} locations={locations} setLocations={setLocations} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} showToast={showToast} onDone={goHome} />}
      </div>
      {toast && <Toast toast={toast} />}
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   PICKER HOME SCREEN
══════════════════════════════════════════════════════════════════════════ */
function PickerHome({ session, onAddStock, onRemoveAisle, onRemoveFloor, onMoveStock }) {
  const [removeOpen, setRemoveOpen] = useState(false);
  const [floorOpen, setFloorOpen] = useState(false);

  return (
    <div style={{ animation: "fadeUp .25s ease" }}>
      {/* Greeting */}
      <div style={{ marginBottom: 32, marginTop: 8 }}>
        <div style={{ color: "#888", fontSize: 15, marginBottom: 2 }}>Good {getGreeting()},</div>
        <div style={{ color: "#fff", fontWeight: 800, fontSize: 30 }}>{session.name}</div>
        <div style={{ color: "#444", fontSize: 13, marginTop: 4 }}>{new Date().toLocaleDateString("en-AU", { weekday: "long", day: "numeric", month: "long" })}</div>
      </div>

      {/* ── ADD STOCK ── */}
      <button onClick={onAddStock} style={{ width: "100%", marginBottom: 16, padding: "24px 22px", background: "linear-gradient(135deg,#0d2208,#1a3a10)", border: `2px solid ${G.green}`, borderRadius: 14, cursor: "pointer", display: "flex", alignItems: "center", gap: 18, textAlign: "left" }}>
        <div style={{ width: 60, height: 60, borderRadius: 14, background: G.green, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 30, flexShrink: 0 }}>↓</div>
        <div style={{ flex: 1 }}>
          <div style={{ color: "#fff", fontWeight: 800, fontSize: 24, letterSpacing: .5 }}>ADD STOCK</div>
          <div style={{ color: "#6db85a", fontSize: 14, marginTop: 3 }}>Scan product → enter details → scan aisle → select bin</div>
        </div>
        <div style={{ color: G.green, fontSize: 28 }}>›</div>
      </button>

      {/* ── REMOVE STOCK ── */}
      <div style={{ background: "linear-gradient(135deg,#220808,#3a1010)", border: `2px solid ${removeOpen ? G.red : "#5a1a1a"}`, borderRadius: 14, overflow: "hidden", transition: "border .2s" }}>
        <button onClick={() => { setRemoveOpen(o => !o); setFloorOpen(false); }} style={{ width: "100%", padding: "24px 22px", background: "none", border: "none", cursor: "pointer", display: "flex", alignItems: "center", gap: 18, textAlign: "left" }}>
          <div style={{ width: 60, height: 60, borderRadius: 14, background: G.red, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 30, color: "#fff", flexShrink: 0 }}>↑</div>
          <div style={{ flex: 1 }}>
            <div style={{ color: "#fff", fontWeight: 800, fontSize: 24, letterSpacing: .5 }}>REMOVE STOCK</div>
            <div style={{ color: "#c47070", fontSize: 14, marginTop: 3 }}>Pick from aisle or floor location</div>
          </div>
          <div style={{ color: G.red, fontSize: 28, transform: removeOpen ? "rotate(90deg)" : "none", transition: "transform .2s" }}>›</div>
        </button>

        {removeOpen && (
          <div style={{ borderTop: "1px solid #4a1a1a", padding: "14px 16px 18px", animation: "fadeUp .15s ease" }}>
            <div style={{ fontSize: 11, color: "#666", letterSpacing: 1, fontWeight: 700, marginBottom: 12 }}>SELECT LOCATION TYPE:</div>

            {/* Aisle option */}
            <button onClick={onRemoveAisle} style={{ width: "100%", padding: "16px 18px", marginBottom: 10, background: "#2a1010", border: "1px solid #5a2020", borderRadius: 10, cursor: "pointer", display: "flex", alignItems: "center", gap: 14 }}>
              <div style={{ width: 44, height: 44, borderRadius: 10, background: "#5a2020", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22, flexShrink: 0 }}>🏭</div>
              <div style={{ textAlign: "left", flex: 1 }}>
                <div style={{ color: "#fff", fontWeight: 700, fontSize: 18 }}>AISLE / BIN</div>
                <div style={{ color: "#c47070", fontSize: 13, marginTop: 1 }}>Scan aisle → select bin → remove</div>
              </div>
              <div style={{ color: "#c47070", fontSize: 22 }}>›</div>
            </button>

            {/* Floor option — expandable */}
            <div style={{ background: "#2a1010", border: `1px solid ${floorOpen ? G.amber : "#5a2020"}`, borderRadius: 10, overflow: "hidden", transition: "border .15s" }}>
              <button onClick={() => setFloorOpen(o => !o)} style={{ width: "100%", padding: "16px 18px", background: "none", border: "none", cursor: "pointer", display: "flex", alignItems: "center", gap: 14 }}>
                <div style={{ width: 44, height: 44, borderRadius: 10, background: "#3a2a10", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22, flexShrink: 0 }}>📦</div>
                <div style={{ textAlign: "left", flex: 1 }}>
                  <div style={{ color: "#fff", fontWeight: 700, fontSize: 18 }}>FLOOR STOCK</div>
                  <div style={{ color: "#c47070", fontSize: 13, marginTop: 1 }}>Select warehouse → scan product → remove</div>
                </div>
                <div style={{ color: G.amber, fontSize: 22, transform: floorOpen ? "rotate(90deg)" : "none", transition: "transform .2s" }}>›</div>
              </button>

              {floorOpen && (
                <div style={{ borderTop: "1px solid #3a2010", padding: "10px 14px 16px", animation: "fadeUp .15s ease" }}>
                  <div style={{ fontSize: 11, color: "#666", letterSpacing: 1, fontWeight: 700, marginBottom: 12 }}>SELECT WAREHOUSE:</div>
                  <div style={{ display: "grid", gap: 8 }}>
                    {WAREHOUSES.map(wh => (
                      <button key={wh} onClick={() => onRemoveFloor(wh)} style={{ padding: "16px 18px", background: "#1a1a0a", border: `2px solid ${G.amber}`, borderRadius: 10, cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "space-between" }}>
                        <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                          <div style={{ width: 10, height: 10, borderRadius: "50%", background: G.amber }} />
                          <span style={{ color: "#fff", fontWeight: 800, fontSize: 18, letterSpacing: .5 }}>{wh.toUpperCase()}</span>
                        </div>
                        <span style={{ color: G.amber, fontSize: 20 }}>›</span>
                      </button>
                    ))}
                  </div>
                </div>
              )}
            </div>
          </div>
        )}
      </div>

      {/* ── MOVE STOCK ── */}
      <button onClick={onMoveStock} style={{ width: "100%", marginTop: 16, padding: "24px 22px", background: "linear-gradient(135deg,#0a0d22,#141a3a)", border: `2px solid #4a6adf`, borderRadius: 14, cursor: "pointer", display: "flex", alignItems: "center", gap: 18, textAlign: "left" }}>
        <div style={{ width: 60, height: 60, borderRadius: 14, background: "#3a50cc", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 28, color: "#fff", flexShrink: 0 }}>⇄</div>
        <div style={{ flex: 1 }}>
          <div style={{ color: "#fff", fontWeight: 800, fontSize: 24, letterSpacing: .5 }}>MOVE STOCK</div>
          <div style={{ color: "#7a9aee", fontSize: 14, marginTop: 3 }}>Aisle → Aisle · Aisle → Floor · Floor → Aisle</div>
        </div>
        <div style={{ color: "#4a6adf", fontSize: 28 }}>›</div>
      </button>
    </div>
  );
}

function getGreeting() {
  const h = new Date().getHours();
  return h < 12 ? "morning" : h < 17 ? "afternoon" : "evening";
}

/* ═══════════════════════════════════════════════════════════════════════════
   ADD STOCK FLOW
══════════════════════════════════════════════════════════════════════════ */
/* ═══════════════════════════════════════════════════════════════════════════
   ADD STOCK — OPTIMISED (3 steps)
   1. Scan product → product shown + BB + Qty + quick-qty buttons on same screen
   2. Scan aisle → auto-selects bin if only 1 free; else tap bin
   3. Confirm (auto-skipped if auto-bin selected)
══════════════════════════════════════════════════════════════════════════ */
const QUICK_QTYS = [12, 24, 48, 96];

function AddStockFlow({ session, products, locations, setLocations, floorStock, setFloorStock, addLog, showToast, onDone }) {
  const [step, setStep]               = useState("scan_product"); // scan_product | details_aisle | select_bin | floor_confirm
  const [productCode, setProductCode] = useState("");
  const [foundProduct, setFoundProduct] = useState(null);
  const [bestBefore, setBestBefore]   = useState("");
  const [qty, setQty]                 = useState("");
  const [aisleCode, setAisleCode]     = useState(() => LS.get("kw_last_aisle","") || "");
  const [aisleBins, setAisleBins]     = useState([]);
  const [selectedBin, setSelectedBin] = useState(null);
  const [floorWarehouse, setFloorWarehouse] = useState(null);
  const [error, setError]             = useState("");
  const productRef = useRef(null);
  const aisleRef   = useRef(null);
  const qtyRef     = useRef(null);

  useEffect(() => { if (step === "scan_product") setTimeout(() => productRef.current?.focus(), 80); }, [step]);
  useEffect(() => { if (step === "details_aisle") setTimeout(() => qtyRef.current?.focus(), 80); }, [step]);

  const lookupProduct = (code) => {
    const p = findProduct(products, code);
    if (p) { setFoundProduct(p); setError(""); setStep("details_aisle"); }
    else { setError(`Product not found. Try product code or barcode.`); setProductCode(""); }
  };

  const lookupAisle = (code) => {
    const c = code.trim().toUpperCase();
    const bins = Object.values(locations).filter(l => l.id.startsWith(c));
    if (!bins.length) { setError(`Aisle "${c}" not found.`); return; }
    const freeBins = bins.filter(l => l.contents.length < l.maxProducts || l.contents.find(x => x.productCode === foundProduct?.code));
    if (!freeBins.length) { setError(`No space in aisle "${c}". Try another.`); return; }
    LS.set("kw_last_aisle", c);
    setAisleBins(bins.sort((a,b) => a.id.localeCompare(b.id)));
    setError("");
    // Auto-select if only 1 free bin
    if (freeBins.length === 1) { setSelectedBin(freeBins[0].id); submitStore(freeBins[0].id, bins); }
    else { setStep("select_bin"); }
  };

  const submitDetails = () => {
    if (!qty || parseInt(qty) <= 0) { setError("Enter quantity."); return; }
    if (!bestBefore) { setError("Enter best before date."); return; }
    setError("");
    // Scan aisle inline if typed
    if (aisleCode.trim()) { lookupAisle(aisleCode); }
    else { setError("Scan or type the aisle code."); setTimeout(() => aisleRef.current?.focus(), 50); }
  };

  const submitStore = (binId, bins) => {
    const locs = bins || aisleBins;
    const loc = locations[binId] || locs.find(l => l.id === binId);
    if (!loc) return;
    const alreadyHas = loc.contents.findIndex(c => c.productCode === foundProduct.code);
    const newContents = alreadyHas >= 0
      ? loc.contents.map((c,i) => i === alreadyHas ? { ...c, qty: c.qty + parseInt(qty), bestBefore, storedAt: new Date().toISOString() } : c)
      : [...loc.contents, { productCode: foundProduct.code, description: foundProduct.description, qty: parseInt(qty), bestBefore, storedAt: new Date().toISOString() }];
    setLocations(prev => ({ ...prev, [loc.id]: { ...loc, contents: newContents } }));
    addLog("STORE", `${foundProduct.description} (${foundProduct.code}) → ${loc.id} | Qty:${qty} | BB:${bestBefore}`, session.username);
    showToast(`✓ Stored ${qty} ctns at ${loc.id}`);
    onDone();
  };

  const confirmFloor = (wh) => { setFloorWarehouse(wh); setStep("floor_confirm"); };

  const saveToFloor = () => {
    const entry = { productCode: foundProduct.code, description: foundProduct.description, qty: parseInt(qty), bestBefore, storedAt: new Date().toISOString() };
    setFloorStock(prev => ({ ...prev, [floorWarehouse]: [...(prev[floorWarehouse] || []), entry] }));
    addLog("FLOOR_STORE", `${foundProduct.description} → FLOOR/${floorWarehouse} | Qty:${qty} | BB:${bestBefore}`, session.username);
    showToast(`✓ Stored on ${floorWarehouse} floor`);
    onDone();
  };

  const STEP_LABELS = ["SCAN PRODUCT", "DETAILS + AISLE", "SELECT BIN"];
  const stepIdx = { scan_product:0, details_aisle:1, select_bin:2, floor_confirm:1 }[step] ?? 0;

  return (
    <div>
      <StepBar steps={STEP_LABELS} current={stepIdx} color={G.green} />

      {/* ── STEP 1: Scan product ── */}
      {step === "scan_product" && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <SectionTitle icon="↓" color={G.green} title="Scan or Search Product" sub="Scan barcode — or type product code manually" />
          <input ref={productRef} className="inp-dark"
            style={{ fontWeight:800, fontSize:22, letterSpacing:2, textAlign:"center" }}
            placeholder="SCAN BARCODE" value={productCode}
            onChange={e => setProductCode(e.target.value)}
            onKeyDown={e => { if (e.key === "Enter" && productCode.trim()) lookupProduct(productCode); }}
            autoComplete="off" />
          <div style={{ display:"flex", alignItems:"center", gap:10, margin:"10px 0" }}>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }}/><span style={{ color:"#555", fontSize:12, fontWeight:700 }}>OR</span><div style={{ flex:1, height:1, background:"#2a2a2a" }}/>
          </div>
          <ProductCodeSearch products={products} onSelect={p => { setFoundProduct(p); setProductCode(p.code); setError(""); setStep("details_aisle"); }} />
          {error && <ErrBox msg={error} />}
          <button className="btn-green-xl" style={{ width:"100%", marginTop:10 }}
            onClick={() => productCode.trim() && lookupProduct(productCode)}>LOOK UP →</button>
        </div>
      )}

      {/* ── STEP 2: Details + Aisle (merged) ── */}
      {step === "details_aisle" && foundProduct && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          {/* Product card compact */}
          <div style={{ background:"#1a1a0a", border:`2px solid ${G.amber}`, borderRadius:10, padding:"12px 16px", marginBottom:16, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
            <div>
              <div style={{ fontSize:11, color:G.amber, fontWeight:700, letterSpacing:1 }}>✓ PRODUCT</div>
              <div style={{ color:"#fff", fontWeight:800, fontSize:18, marginTop:2 }}>{foundProduct.description}</div>
              <div style={{ color:"#888", fontSize:12, fontFamily:"monospace" }}>{foundProduct.code}</div>
            </div>
            <button onClick={() => { setStep("scan_product"); setFoundProduct(null); setProductCode(""); }}
              style={{ background:"none", border:"1px solid #444", color:"#888", borderRadius:6, padding:"6px 12px", cursor:"pointer", fontFamily:"'Barlow Condensed',sans-serif", fontSize:13 }}>CHANGE</button>
          </div>

          {/* QTY + Quick buttons */}
          <label className="lbl-dark">QUANTITY (CTNS)</label>
          <input ref={qtyRef} className="inp-dark" type="number" min="1" value={qty}
            onChange={e => setQty(e.target.value)} placeholder="0"
            style={{ fontSize:28, fontWeight:800, textAlign:"center", minHeight:64 }} />
          <div style={{ display:"flex", gap:8, marginTop:8 }}>
            {QUICK_QTYS.map(q => (
              <button key={q} onClick={() => setQty(String(q))}
                style={{ flex:1, padding:"10px 0", background: qty===String(q) ? G.green : "#1e1e1e", border:`2px solid ${qty===String(q) ? G.green : "#333"}`, borderRadius:8, color: qty===String(q) ? "#fff" : "#888", fontWeight:800, fontSize:16, cursor:"pointer", fontFamily:"'Barlow Condensed',sans-serif", transition:"all .15s" }}>
                {q}
              </button>
            ))}
          </div>

          {/* Best before */}
          <label className="lbl-dark" style={{ marginTop:16 }}>BEST BEFORE DATE</label>
          <input className="inp-dark" type="date" value={bestBefore}
            onChange={e => setBestBefore(e.target.value)} style={{ fontSize:17, minHeight:52 }} />

          {/* Aisle inline */}
          <label className="lbl-dark" style={{ marginTop:16 }}>AISLE BARCODE</label>
          <div style={{ display:"flex", gap:8 }}>
            <input ref={aisleRef} className="inp-dark"
              style={{ flex:1, textTransform:"uppercase", fontWeight:800, fontSize:22, letterSpacing:4, textAlign:"center" }}
              placeholder="J01" value={aisleCode}
              onChange={e => setAisleCode(e.target.value.toUpperCase())}
              onKeyDown={e => { if (e.key === "Enter") submitDetails(); }}
              autoComplete="off" />
            {aisleCode && <div style={{ display:"flex", alignItems:"center", background:"#1a1a0a", border:`1px solid ${G.amber}`, borderRadius:8, padding:"0 12px", color:G.amber, fontSize:11, fontWeight:700, letterSpacing:1, whiteSpace:"nowrap" }}>LAST: {aisleCode}</div>}
          </div>
          {error && <ErrBox msg={error} />}

          <button className="btn-green-xl" style={{ width:"100%", marginTop:14 }} onClick={submitDetails}>
            CHECK AISLE + SAVE →
          </button>

          {/* Floor divider */}
          <div style={{ display:"flex", alignItems:"center", gap:10, margin:"14px 0 10px" }}>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
            <span style={{ color:"#555", fontSize:13, fontWeight:700 }}>OR STORE ON FLOOR</span>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
          </div>
          <FloorWarehousePicker onSelect={confirmFloor} />
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:12 }}
            onClick={() => { setStep("scan_product"); setFoundProduct(null); setProductCode(""); }}>← BACK</button>
        </div>
      )}

      {/* ── STEP 3: Select bin (only if multiple free bins) ── */}
      {step === "select_bin" && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <SummaryBar product={foundProduct} bestBefore={bestBefore} qty={qty} color={G.green} label="ADDING" />
          <div style={{ color:"#fff", fontWeight:800, fontSize:20, marginBottom:4 }}>Select Bin</div>
          <div style={{ color:"#aaa", fontSize:14, marginBottom:14 }}>
            Aisle <span style={{ color:G.amber, fontFamily:"monospace", fontWeight:700 }}>{aisleCode}</span> — tap a bin slot
          </div>
          <div style={{ display:"grid", gap:10 }}>
            {aisleBins.map(loc => {
              const free = loc.maxProducts - loc.contents.length;
              const hasProd = loc.contents.find(c => c.productCode === foundProduct.code);
              const isFull = free === 0 && !hasProd;
              // Allowed products check — only warn if admin set allowed list AND this product not in it
              const hasAllowed = loc.allowedProducts?.length > 0;
              const notAllowed = hasAllowed && !loc.allowedProducts.includes(foundProduct?.code);
              return (
                <div key={loc.id}>
                  <button onClick={() => { if (!isFull) submitStore(loc.id, aisleBins); }}
                    style={{ width:"100%", padding:"14px 16px", borderRadius:10, border:`2px solid ${isFull?"#2a1010":hasProd?G.amber:G.green}`, background:isFull?"#111":hasProd?"#1a1800":"#0d1f0d", cursor:isFull?"not-allowed":"pointer", textAlign:"left", opacity:isFull?.45:1, display:"flex", alignItems:"center", justifyContent:"space-between" }}>
                    <div>
                      <div style={{ display:"flex", alignItems:"center", gap:8, marginBottom:4, flexWrap:"wrap" }}>
                        <span style={{ fontFamily:"monospace", fontWeight:800, fontSize:20, color:isFull?"#555":"#fff" }}>{loc.id}</span>
                        {loc.customName && <span style={{ fontSize:12, color:"#aaa" }}>{loc.customName}</span>}
                        {hasProd && <span style={{ background:G.amberLight, color:G.amberDark, borderRadius:4, padding:"1px 7px", fontSize:11, fontWeight:700 }}>HAS PRODUCT</span>}
                        {isFull  && <span style={{ background:"#2a0808", color:G.red, borderRadius:4, padding:"1px 7px", fontSize:11, fontWeight:700 }}>FULL</span>}
                      </div>
                      {loc.contents.length === 0
                        ? <div style={{ fontSize:13, color:"#4a8a4a" }}>Empty — tap to store here</div>
                        : loc.contents.map((c,i) => <div key={i} style={{ fontSize:12, color:"#aaa" }}>{c.description} · {c.qty} ctns</div>)
                      }
                    </div>
                    <div style={{ display:"flex", gap:4, flexShrink:0, marginLeft:10 }}>
                      {Array.from({length:loc.maxProducts}).map((_,i) => (
                        <div key={i} style={{ width:22, height:22, borderRadius:5, background:i<loc.contents.length?G.amber:"#1e3a1e", border:`2px solid ${i<loc.contents.length?G.amberDark:"#2a5a2a"}`, display:"flex", alignItems:"center", justifyContent:"center", fontSize:10, color:"#fff" }}>
                          {i<loc.contents.length?"■":"○"}
                        </div>
                      ))}
                    </div>
                  </button>
                  {notAllowed && !isFull && (
                    <div style={{ background:"#2a1500", border:"1px solid #c47000", borderRadius:"0 0 8px 8px", padding:"8px 14px", marginTop:-2, fontSize:13, color:"#f0a030", fontWeight:700 }}>
                      ⚠ This product is not allowed in this aisle
                    </div>
                  )}
                </div>
              );
            })}
          </div>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:14 }} onClick={() => setStep("details_aisle")}>← BACK</button>
        </div>
      )}

      {/* ── Floor confirm ── */}
      {step === "floor_confirm" && floorWarehouse && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <div style={{ background:"#1a1a0a", border:`2px solid ${G.amber}`, borderRadius:12, padding:"20px", marginBottom:20 }}>
            <div style={{ fontSize:11, color:G.amber, letterSpacing:2, fontWeight:700, marginBottom:8 }}>✓ CONFIRM FLOOR STORAGE</div>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:14 }}>
              <div>
                <div style={{ color:"#fff", fontWeight:800, fontSize:20 }}>{foundProduct.description}</div>
                <div style={{ color:"#aaa", fontSize:13, marginTop:2 }}>Best Before: {bestBefore}</div>
              </div>
              <div style={{ textAlign:"right" }}>
                <div style={{ color:"#fff", fontWeight:800, fontSize:38, lineHeight:1 }}>{qty}</div>
                <div style={{ color:"#aaa", fontSize:13 }}>ctns</div>
              </div>
            </div>
            <div style={{ background:"#111", borderRadius:8, padding:"12px 16px", display:"flex", alignItems:"center", gap:10 }}>
              <span style={{ fontSize:20 }}>📦</span>
              <div>
                <div style={{ fontSize:11, color:"#666", letterSpacing:1 }}>FLOOR</div>
                <div style={{ color:G.amber, fontWeight:800, fontSize:20 }}>{floorWarehouse.toUpperCase()}</div>
              </div>
            </div>
          </div>
          <button className="btn-amber-xl" style={{ width:"100%", marginBottom:10 }} onClick={saveToFloor}>✓ SAVE TO FLOOR</button>
          <button className="btn-outline-xl" style={{ width:"100%" }} onClick={() => { setStep("details_aisle"); setFloorWarehouse(null); }}>← BACK</button>
        </div>
      )}
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   REMOVE STOCK — OPTIMISED (2–3 steps)
   1. Scan aisle → select bin (auto if 1 bin with stock)
   2. Select product if 2+ → merged qty+confirm on one screen
   Qty input + Take All button + Confirm all on same screen = 1 fewer tap
══════════════════════════════════════════════════════════════════════════ */
function RemoveAisleFlow({ session, locations, setLocations, addLog, showToast, onDone }) {
  const [step, setStep]             = useState("scan_aisle");
  const [aisleCode, setAisleCode]   = useState("");
  const [aisleBins, setAisleBins]   = useState([]);
  const [selectedBin, setSelectedBin] = useState(null);
  const [selectedIdx, setSelectedIdx] = useState(null);
  const [removeQty, setRemoveQty]   = useState("");
  const [error, setError]           = useState("");
  const aisleRef = useRef(null);

  useEffect(() => { if (step === "scan_aisle") setTimeout(() => aisleRef.current?.focus(), 80); }, [step]);

  const currentLoc  = selectedBin ? locations[selectedBin] : null;
  const currentItem = currentLoc && selectedIdx !== null ? currentLoc.contents[selectedIdx] : null;

  const lookupAisle = (code) => {
    const c = code.trim().toUpperCase();
    const bins = Object.values(locations).filter(l => l.id.startsWith(c));
    if (!bins.length) { setError(`Aisle "${c}" not found.`); setAisleCode(""); return; }
    const withStock = bins.filter(b => b.contents.length > 0).sort((a,b) => a.id.localeCompare(b.id));
    if (!withStock.length) { setError(`No stock in aisle "${c}".`); setAisleCode(""); return; }
    setAisleBins(withStock); setError("");
    // Auto-select if only 1 bin has stock
    if (withStock.length === 1) selectBin(withStock[0].id, withStock);
    else setStep("select_bin");
  };

  const selectBin = (binId, bins) => {
    const allBins = bins || aisleBins;
    const loc = locations[binId] || allBins.find(l => l.id === binId);
    if (!loc?.contents.length) return;
    setSelectedBin(binId);
    if (loc.contents.length === 1) { setSelectedIdx(0); setStep("remove_qty"); }
    else setStep("select_product");
  };

  const doRemove = (all = false) => {
    const n = all ? currentItem.qty : parseInt(removeQty);
    if (!all && (!n || n <= 0 || n > currentItem.qty)) { setError(`Enter 1–${currentItem.qty}.`); return; }
    const newQty = currentItem.qty - n;
    const newContents = newQty === 0
      ? currentLoc.contents.filter((_,i) => i !== selectedIdx)
      : currentLoc.contents.map((c,i) => i === selectedIdx ? { ...c, qty:newQty } : c);
    setLocations(prev => ({ ...prev, [selectedBin]: { ...currentLoc, contents:newContents } }));
    addLog(all?"REMOVE_ALL":"REMOVE", `${currentItem.description} ← ${selectedBin} | Removed:${n} | Left:${newQty}`, session.username);
    showToast(`✓ Removed ${n} ctns from ${selectedBin}`);
    onDone();
  };

  const STEPS = ["SCAN AISLE","SELECT BIN","REMOVE"];
  const stepIdx = { scan_aisle:0, select_bin:1, select_product:1, remove_qty:2 }[step] ?? 0;

  return (
    <div>
      <StepBar steps={STEPS} current={stepIdx} color={G.red} />

      {step === "scan_aisle" && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <SectionTitle icon="🏭" color={G.red} title="Scan Aisle" sub="Scan or type aisle code (e.g. J01)" />
          <input ref={aisleRef} className="inp-dark"
            style={{ textTransform:"uppercase", fontWeight:800, fontSize:28, letterSpacing:6, textAlign:"center" }}
            placeholder="J01" value={aisleCode}
            onChange={e => setAisleCode(e.target.value.toUpperCase())}
            onKeyDown={e => { if (e.key==="Enter" && aisleCode.trim()) lookupAisle(aisleCode); }}
            autoComplete="off" />
          {error && <ErrBox msg={error} />}
          <button className="btn-red-xl" style={{ width:"100%", marginTop:14 }}
            onClick={() => aisleCode.trim() && lookupAisle(aisleCode)}>CHECK AISLE →</button>
        </div>
      )}

      {step === "select_bin" && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <div style={{ color:"#fff", fontWeight:800, fontSize:20, marginBottom:4 }}>Select Bin</div>
          <div style={{ color:"#aaa", fontSize:14, marginBottom:14 }}>Aisle <span style={{ color:G.amber, fontFamily:"monospace", fontWeight:700 }}>{aisleCode}</span></div>
          <div style={{ display:"grid", gap:10 }}>
            {aisleBins.map(loc => (
              <button key={loc.id} onClick={() => selectBin(loc.id)}
                style={{ padding:"14px 16px", borderRadius:10, border:`2px solid ${G.red}`, background:"#1a0808", cursor:"pointer", textAlign:"left", display:"flex", alignItems:"center", justifyContent:"space-between" }}>
                <div>
                  <div style={{ fontFamily:"monospace", fontWeight:800, fontSize:20, color:"#fff", marginBottom:4 }}>{loc.id}</div>
                  {loc.contents.map((c,i) => (
                    <div key={i} style={{ fontSize:13, color:"#aaa" }}>
                      <span style={{ color:"#ddd", fontWeight:600 }}>{c.description}</span> · <span style={{ color:G.red, fontWeight:700 }}>{c.qty} ctns</span>
                    </div>
                  ))}
                </div>
                <div style={{ color:G.red, fontSize:24, marginLeft:12 }}>›</div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:14 }}
            onClick={() => { setStep("scan_aisle"); setAisleCode(""); }}>← SCAN DIFFERENT AISLE</button>
        </div>
      )}

      {step === "select_product" && currentLoc && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <div style={{ color:"#fff", fontWeight:800, fontSize:20, marginBottom:4 }}>Select Product</div>
          <div style={{ color:"#aaa", fontSize:14, marginBottom:14 }}>Bin <span style={{ color:G.amber, fontFamily:"monospace", fontWeight:700 }}>{selectedBin}</span></div>
          <div style={{ display:"grid", gap:10 }}>
            {currentLoc.contents.map((c,i) => (
              <button key={i} onClick={() => { setSelectedIdx(i); setStep("remove_qty"); }}
                style={{ padding:"16px 18px", borderRadius:10, border:`2px solid ${G.red}`, background:"#1a0808", cursor:"pointer", textAlign:"left", display:"flex", justifyContent:"space-between", alignItems:"center" }}>
                <div>
                  <div style={{ color:"#fff", fontWeight:800, fontSize:18 }}>{c.description}</div>
                  <div style={{ color:"#aaa", fontSize:13, marginTop:2 }}>BB: {c.bestBefore||"—"}</div>
                </div>
                <div style={{ textAlign:"right" }}>
                  <div style={{ color:G.red, fontWeight:800, fontSize:28, lineHeight:1 }}>{c.qty}</div>
                  <div style={{ color:"#aaa", fontSize:12 }}>ctns</div>
                </div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:14 }} onClick={() => setStep("select_bin")}>← BACK</button>
        </div>
      )}

      {/* Merged qty + confirm on one screen */}
      {step === "remove_qty" && currentItem && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <div style={{ background:"#1a0808", border:`2px solid ${G.red}`, borderRadius:12, padding:"16px 18px", marginBottom:16 }}>
            <div style={{ fontSize:11, color:G.red, fontWeight:700, letterSpacing:2, marginBottom:4 }}>FROM {selectedBin}</div>
            <div style={{ color:"#fff", fontWeight:800, fontSize:20 }}>{currentItem.description}</div>
            <div style={{ color:"#aaa", fontSize:13, marginTop:2 }}>BB: {currentItem.bestBefore||"—"}</div>
          </div>

          {/* Big TAKE ALL button first — fastest path */}
          <button className="btn-red-xl" style={{ width:"100%", marginBottom:12 }} onClick={() => doRemove(true)}>
            ✓ TAKE ALL — {currentItem.qty} ctns
          </button>

          <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:12 }}>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
            <span style={{ color:"#555", fontSize:13, fontWeight:700 }}>OR PARTIAL</span>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
          </div>

          <input className="inp-dark" type="number" min="1" max={currentItem.qty} value={removeQty}
            onChange={e => setRemoveQty(e.target.value)} placeholder={`1–${currentItem.qty}`}
            style={{ fontSize:24, fontWeight:800, textAlign:"center", minHeight:58 }} autoFocus />
          {error && <ErrBox msg={error} />}
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:10, borderColor:"#c47070", color:"#c47070" }}
            onClick={() => doRemove(false)}>REMOVE THIS AMOUNT</button>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:10 }}
            onClick={() => setStep(currentLoc?.contents.length > 1 ? "select_product" : "select_bin")}>← BACK</button>
        </div>
      )}
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   REMOVE STOCK — FLOOR (unchanged, already optimal)
══════════════════════════════════════════════════════════════════════════ */
function RemoveFloorFlow({ session, warehouse, products, floorStock, setFloorStock, addLog, showToast, onDone }) {
  const [step, setStep]               = useState("scan_product");
  const [scanCode, setScanCode]       = useState("");
  const [matchedCode, setMatchedCode] = useState(null);
  const [selectedIdx, setSelectedIdx] = useState(null);
  const [removeQty, setRemoveQty]     = useState("");
  const [error, setError]             = useState("");
  const scanRef = useRef(null);

  useEffect(() => { if (step === "scan_product") setTimeout(() => scanRef.current?.focus(), 80); }, [step]);

  const warehouseStock = floorStock[warehouse] || [];
  const matchedEntries = matchedCode
    ? warehouseStock.map((e,i) => ({...e,_idx:i})).filter(e => e.productCode === matchedCode)
    : [];
  const selectedEntry = selectedIdx !== null ? warehouseStock[selectedIdx] : null;

  const lookupProduct = (code) => {
    const inCatalogue = findProduct(products, code);
    if (!inCatalogue) { setError(`Product not found. Try product code or barcode.`); setScanCode(""); return; }
    const onFloor = warehouseStock.filter(e => e.productCode === c);
    if (!onFloor.length) { setError(`No floor stock for "${inCatalogue.description}" at ${warehouse}.`); setScanCode(""); return; }
    setMatchedCode(c); setError("");
    if (onFloor.length === 1) { setSelectedIdx(warehouseStock.findIndex(e => e.productCode === c)); setStep("confirm_remove"); }
    else setStep("select_entry");
  };

  const doRemove = (all = false) => {
    const entry = selectedEntry;
    const n = all ? entry.qty : parseInt(removeQty);
    if (!all && (!n || n <= 0 || n > entry.qty)) { setError(`Enter 1–${entry.qty}.`); return; }
    const newQty = entry.qty - n;
    const newStock = newQty === 0
      ? warehouseStock.filter((_,i) => i !== selectedIdx)
      : warehouseStock.map((e,i) => i === selectedIdx ? {...e, qty:newQty} : e);
    setFloorStock(prev => ({...prev, [warehouse]: newStock}));
    addLog(all?"FLOOR_REMOVE_ALL":"FLOOR_REMOVE",
      `${entry.description} ← FLOOR/${warehouse} | Removed:${n} | Left:${newQty}`, session.username);
    showToast(`✓ Removed ${n} ctns from ${warehouse} floor`);
    onDone();
  };

  const STEPS = ["SCAN PRODUCT","SELECT BATCH","REMOVE"];
  const stepIdx = { scan_product:0, select_entry:1, confirm_remove:2 }[step] ?? 0;
  const barSteps = matchedEntries.length <= 1 ? ["SCAN PRODUCT","REMOVE"] : STEPS;
  const barIdx   = matchedEntries.length <= 1 ? Math.min(stepIdx,1) : stepIdx;

  return (
    <div>
      <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:16 }}>
        <div style={{ background:G.amberLight, border:`2px solid ${G.amber}`, borderRadius:8, padding:"6px 16px", display:"inline-flex", alignItems:"center", gap:8 }}>
          <span>📦</span>
          <span style={{ fontWeight:800, fontSize:16, color:G.amberDark }}>{warehouse.toUpperCase()}</span>
          <span style={{ color:G.muted, fontSize:13 }}>FLOOR</span>
        </div>
        <div style={{ color:"#555", fontSize:13 }}>{warehouseStock.length} entries</div>
      </div>
      <StepBar steps={barSteps} current={barIdx} color={G.amber} />

      {step === "scan_product" && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          {warehouseStock.length === 0 ? (
            <div style={{ background:"#111", border:"2px solid #333", borderRadius:12, padding:"36px 24px", textAlign:"center" }}>
              <div style={{ fontSize:48, marginBottom:12 }}>📭</div>
              <div style={{ color:"#fff", fontWeight:800, fontSize:22, marginBottom:8 }}>No Floor Stock</div>
              <div style={{ color:"#888", fontSize:15, marginBottom:28 }}>Nothing on floor at <span style={{ color:G.amber, fontWeight:700 }}>{warehouse}</span>.</div>
              <button className="btn-outline-xl" style={{ width:"100%" }} onClick={onDone}>← BACK</button>
            </div>
          ) : (
            <>
              <SectionTitle icon="📦" color={G.amber} title="Scan or Search Product" sub={`Floor stock at ${warehouse}`} />
              <div style={{ background:"#1a1a0a", border:"1px solid #333", borderRadius:10, padding:"12px 16px", marginBottom:16 }}>
                <div style={{ fontSize:11, color:"#666", letterSpacing:1, fontWeight:700, marginBottom:8 }}>ON FLOOR:</div>
                {Object.entries(warehouseStock.reduce((acc,e) => {
                  if (!acc[e.productCode]) acc[e.productCode] = { description:e.description, total:0 };
                  acc[e.productCode].total += e.qty; return acc;
                }, {})).map(([code,info]) => (
                  <div key={code} style={{ display:"flex", justifyContent:"space-between", padding:"6px 0", borderBottom:"1px solid #222" }}>
                    <div>
                      <div style={{ color:"#ddd", fontWeight:700, fontSize:14 }}>{info.description}</div>
                      <div style={{ color:"#555", fontSize:12, fontFamily:"monospace" }}>{code}</div>
                    </div>
                    <div style={{ color:G.amber, fontWeight:800, fontSize:18 }}>{info.total}</div>
                  </div>
                ))}
              </div>
              <input ref={scanRef} className="inp-dark"
                style={{ textTransform:"uppercase", fontWeight:800, fontSize:22, letterSpacing:3, textAlign:"center" }}
                placeholder="SCAN PRODUCT CODE" value={scanCode}
                onChange={e => setScanCode(e.target.value.toUpperCase())}
                onKeyDown={e => { if (e.key==="Enter" && scanCode.trim()) lookupProduct(scanCode); }}
                autoComplete="off" />
              {error && <ErrBox msg={error} />}
              <button className="btn-amber-xl" style={{ width:"100%", marginTop:14 }}
                onClick={() => scanCode.trim() && lookupProduct(scanCode)}>FIND PRODUCT →</button>
            </>
          )}
        </div>
      )}

      {step === "select_entry" && matchedCode && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <ProductFoundCard product={products.find(p=>p.code===matchedCode)||{code:matchedCode,description:matchedEntries[0]?.description}} />
          <div style={{ color:"#fff", fontWeight:800, fontSize:18, margin:"14px 0 6px" }}>Select Batch</div>
          <div style={{ display:"grid", gap:10 }}>
            {matchedEntries.map(e => (
              <button key={e._idx} onClick={() => { setSelectedIdx(e._idx); setStep("confirm_remove"); }}
                style={{ padding:"14px 16px", borderRadius:10, border:`2px solid ${G.amber}`, background:"#1a1600", cursor:"pointer", textAlign:"left", display:"flex", justifyContent:"space-between", alignItems:"center" }}>
                <div>
                  <div style={{ color:"#aaa", fontSize:11, fontWeight:700, letterSpacing:1 }}>BEST BEFORE</div>
                  <div style={{ color:"#fff", fontWeight:800, fontSize:18, marginTop:2 }}>{e.bestBefore||"No date"}</div>
                </div>
                <div style={{ textAlign:"right" }}>
                  <div style={{ color:G.amber, fontWeight:800, fontSize:26 }}>{e.qty}</div>
                  <div style={{ color:"#aaa", fontSize:12 }}>ctns</div>
                </div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:14 }}
            onClick={() => { setStep("scan_product"); setMatchedCode(null); setScanCode(""); }}>← BACK</button>
        </div>
      )}

      {step === "confirm_remove" && selectedEntry && (
        <div style={{ animation:"fadeUp .2s ease" }}>
          <div style={{ background:"#1a0d00", border:`2px solid ${G.amber}`, borderRadius:12, padding:"16px 18px", marginBottom:16 }}>
            <div style={{ fontSize:11, color:G.amber, letterSpacing:2, fontWeight:700, marginBottom:6 }}>REMOVING FROM {warehouse.toUpperCase()} FLOOR</div>
            <div style={{ color:"#fff", fontWeight:800, fontSize:20 }}>{selectedEntry.description}</div>
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:8, marginTop:10 }}>
              <div style={{ background:"#111", borderRadius:6, padding:"8px 12px" }}>
                <div style={{ fontSize:11, color:"#666", letterSpacing:1 }}>BEST BEFORE</div>
                <div style={{ color:"#fff", fontWeight:700, fontSize:15, marginTop:2 }}>{selectedEntry.bestBefore||"—"}</div>
              </div>
              <div style={{ background:"#111", borderRadius:6, padding:"8px 12px" }}>
                <div style={{ fontSize:11, color:"#666", letterSpacing:1 }}>IN STOCK</div>
                <div style={{ color:G.amber, fontWeight:800, fontSize:22, marginTop:2 }}>{selectedEntry.qty} ctns</div>
              </div>
            </div>
          </div>
          <button className="btn-amber-xl" style={{ width:"100%", marginBottom:10 }} onClick={() => doRemove(true)}>
            ✓ TAKE ALL — {selectedEntry.qty} ctns
          </button>
          <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:10 }}>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
            <span style={{ color:"#555", fontSize:13, fontWeight:700 }}>OR PARTIAL</span>
            <div style={{ flex:1, height:1, background:"#2a2a2a" }} />
          </div>
          <input className="inp-dark" type="number" min="1" max={selectedEntry.qty} value={removeQty}
            onChange={e => setRemoveQty(e.target.value)} placeholder={`1–${selectedEntry.qty}`}
            style={{ fontSize:24, fontWeight:800, textAlign:"center", minHeight:58 }} autoFocus />
          {error && <ErrBox msg={error} />}
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:10, borderColor:"#c47070", color:"#c47070" }}
            onClick={() => doRemove(false)}>REMOVE THIS AMOUNT</button>
          <button className="btn-outline-xl" style={{ width:"100%", marginTop:10 }}
            onClick={() => { if (matchedEntries.length>1) setStep("select_entry"); else { setStep("scan_product"); setMatchedCode(null); setScanCode(""); } }}>← BACK</button>
        </div>
      )}
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════════════════════
   MOVE STOCK — OPTIMISED (2 steps)
   All sub-modes merged qty+destination on one screen
══════════════════════════════════════════════════════════════════════════ */
function MoveStockFlow({ session, products, locations, setLocations, floorStock, setFloorStock, addLog, showToast, onDone }) {
  const [subMode, setSubMode] = useState(null);
  if (!subMode) return <MoveHome onSelect={setSubMode} />;
  if (subMode === "aisle_to_aisle" || subMode === "aisle_to_floor")
    return <MoveFromAisle session={session} subMode={subMode} locations={locations} setLocations={setLocations} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} showToast={showToast} onDone={onDone} onBack={() => setSubMode(null)} />;
  if (subMode === "floor_to_aisle")
    return <MoveFromFloor session={session} products={products} locations={locations} setLocations={setLocations} floorStock={floorStock} setFloorStock={setFloorStock} addLog={addLog} showToast={showToast} onDone={onDone} onBack={() => setSubMode(null)} />;
  return null;
}

function MoveHome({ onSelect }) {
  const BLUE = "#3a50cc";
  return (
    <div style={{ animation:"fadeUp .2s ease" }}>
      <div style={{ color:"#fff", fontWeight:800, fontSize:22, marginBottom:4 }}>Move Stock</div>
      <div style={{ color:"#888", fontSize:14, marginBottom:20 }}>Select move type — 2 steps each</div>
      <div style={{ display:"grid", gap:12 }}>
        {[
          { id:"aisle_to_aisle", icon:"🏭→🏭", label:"AISLE → AISLE",  sub:"Pick product + qty · scan TO aisle · done" },
          { id:"aisle_to_floor", icon:"🏭→📦", label:"AISLE → FLOOR",  sub:"Pick product + qty · choose warehouse · done" },
          { id:"floor_to_aisle", icon:"📦→🏭", label:"FLOOR → AISLE",  sub:"Scan product · pick qty · scan TO aisle · done" },
        ].map(o => (
          <button key={o.id} onClick={() => onSelect(o.id)}
            style={{ padding:"20px 18px", borderRadius:12, border:`2px solid ${BLUE}`, background:"#0a0d22", cursor:"pointer", textAlign:"left", display:"flex", alignItems:"center", gap:16 }}>
            <div style={{ width:52, height:52, borderRadius:12, background:BLUE, display:"flex", alignItems:"center", justifyContent:"center", fontSize:22, flexShrink:0 }}>{o.icon}</div>
            <div style={{ flex:1 }}>
              <div style={{ color:"#fff", fontWeight:800, fontSize:18 }}>{o.label}</div>
              <div style={{ color:"#7a9aee", fontSize:13, marginTop:3 }}>{o.sub}</div>
            </div>
            <div style={{ color:BLUE, fontSize:22 }}>›</div>
          </button>
        ))}
      </div>
    </div>
  );
}

/* ── Move FROM Aisle (optimised) ── */
function MoveFromAisle({ session, subMode, locations, setLocations, floorStock, setFloorStock, addLog, showToast, onDone, onBack }) {
  const BLUE = "#3a50cc";
  // Steps: scan_from → select_from_bin → select_product → dest+qty → confirm
  const [step, setStep]               = useState("scan_from");
  const [fromAisle, setFromAisle]     = useState("");
  const [fromBins, setFromBins]       = useState([]);
  const [fromBin, setFromBin]         = useState(null);
  const [fromProdIdx, setFromProdIdx] = useState(null);
  const [moveQty, setMoveQty]         = useState("");
  const [toAisle, setToAisle]         = useState("");
  const [toBin, setToBin]             = useState(null);
  const [toWarehouse, setToWarehouse] = useState(null);
  const [aisleToBins, setAisleToBins] = useState([]);
  const [error, setError]             = useState("");
  const fromRef = useRef(null); const toRef = useRef(null);

  useEffect(() => { if (step==="scan_from") setTimeout(()=>fromRef.current?.focus(),80); },[step]);
  useEffect(() => { if (step==="dest_qty")  setTimeout(()=>toRef.current?.focus(),80); },[step]);

  const fromLocObj = fromBin ? locations[fromBin] : null;
  const fromItem   = fromLocObj && fromProdIdx!==null ? fromLocObj.contents[fromProdIdx] : null;
  const toBinsFiltered = aisleToBins.filter(l => {
    const hasProd = l.contents.find(c=>c.productCode===fromItem?.productCode);
    return hasProd || l.contents.length < l.maxProducts;
  });

  const lookupFromAisle = (code) => {
    const c = code.trim().toUpperCase();
    const bins = Object.values(locations).filter(l=>l.id.startsWith(c)&&l.contents.length>0).sort((a,b)=>a.id.localeCompare(b.id));
    if (!bins.length) { setError(`No stock in "${c}".`); setFromAisle(""); return; }
    setFromBins(bins); setFromAisle(c); setError("");
    if (bins.length===1) selectFromBin(bins[0].id, bins);
    else setStep("select_from_bin");
  };

  const selectFromBin = (binId, bins) => {
    const loc = (bins||fromBins).find(l=>l.id===binId) || locations[binId];
    setFromBin(binId);
    if (loc.contents.length===1) { setFromProdIdx(0); setStep("dest_qty"); }
    else setStep("select_product");
  };

  const lookupToAisle = (code) => {
    const c = code.trim().toUpperCase();
    const bins = Object.values(locations).filter(l=>l.id.startsWith(c)).sort((a,b)=>a.id.localeCompare(b.id));
    if (!bins.length) { setError(`Aisle "${c}" not found.`); setToAisle(""); return; }
    const free = bins.filter(l=>{ const hp=l.contents.find(c2=>c2.productCode===fromItem?.productCode); return hp||l.contents.length<l.maxProducts; });
    if (!free.length) { setError(`No space in "${c}".`); setToAisle(""); return; }
    setAisleToBins(bins); setToAisle(c); setError("");
    // Auto-select if only 1 free bin
    if (free.length===1) { setToBin(free[0].id); setStep("confirm"); }
    else setStep("select_to_bin");
  };

  const validateQty = () => {
    const n = parseInt(moveQty);
    if (!n||n<=0||n>fromItem.qty) { setError(`Enter 1–${fromItem.qty}.`); return false; }
    return true;
  };

  const confirmMove = () => {
    const n = parseInt(moveQty);
    const newFromQty = fromItem.qty - n;
    const newFromContents = newFromQty===0
      ? fromLocObj.contents.filter((_,i)=>i!==fromProdIdx)
      : fromLocObj.contents.map((c,i)=>i===fromProdIdx?{...c,qty:newFromQty}:c);

    if (subMode==="aisle_to_floor") {
      const entry={productCode:fromItem.productCode,description:fromItem.description,qty:n,bestBefore:fromItem.bestBefore,storedAt:new Date().toISOString()};
      setFloorStock(prev=>({...prev,[toWarehouse]:[...(prev[toWarehouse]||[]),entry]}));
      setLocations(prev=>({...prev,[fromBin]:{...fromLocObj,contents:newFromContents}}));
      addLog("MOVE",`${fromItem.description} · ${fromBin} → FLOOR/${toWarehouse} · Qty:${n}`,session.username);
      showToast(`✓ Moved ${n} ctns to ${toWarehouse} floor`);
    } else {
      const toLoc=locations[toBin];
      const hp=toLoc.contents.findIndex(c=>c.productCode===fromItem.productCode);
      const newToContents = hp>=0
        ? toLoc.contents.map((c,i)=>i===hp?{...c,qty:c.qty+n}:c)
        : [...toLoc.contents,{productCode:fromItem.productCode,description:fromItem.description,qty:n,bestBefore:fromItem.bestBefore,storedAt:new Date().toISOString()}];
      setLocations(prev=>({...prev,[fromBin]:{...fromLocObj,contents:newFromContents},[toBin]:{...toLoc,contents:newToContents}}));
      addLog("MOVE",`${fromItem.description} · ${fromBin} → ${toBin} · Qty:${n}`,session.username);
      showToast(`✓ Moved ${n} ctns: ${fromBin} → ${toBin}`);
    }
    onDone();
  };

  const STEPS_AA=["FROM","PRODUCT","QTY + TO","CONFIRM"];
  const STEPS_AF=["FROM","PRODUCT","QTY + FLOOR","CONFIRM"];
  const STEPS=subMode==="aisle_to_floor"?STEPS_AF:STEPS_AA;
  const stepMap={scan_from:0,select_from_bin:0,select_product:1,dest_qty:2,select_to_bin:2,confirm:3};
  const stepIdx=stepMap[step]??0;

  return (
    <div>
      <StepBar steps={STEPS} current={stepIdx} color={BLUE} />

      {step==="scan_from" && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <SectionTitle icon="⇄" color={BLUE} title="Scan FROM Aisle" sub="Scan the aisle you're moving stock out of" />
          <input ref={fromRef} className="inp-dark" style={{textTransform:"uppercase",fontWeight:800,fontSize:28,letterSpacing:6,textAlign:"center"}}
            placeholder="J01" value={fromAisle} onChange={e=>setFromAisle(e.target.value.toUpperCase())}
            onKeyDown={e=>{if(e.key==="Enter"&&fromAisle.trim())lookupFromAisle(fromAisle);}} autoComplete="off" />
          {error&&<ErrBox msg={error}/>}
          <button className="btn-xl-blue" style={{width:"100%",marginTop:14}} onClick={()=>fromAisle.trim()&&lookupFromAisle(fromAisle)}>CHECK AISLE →</button>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:10}} onClick={onBack}>← BACK</button>
        </div>
      )}

      {step==="select_from_bin" && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:4}}>Select Source Bin</div>
          <div style={{color:"#aaa",fontSize:14,marginBottom:14}}>Aisle <span style={{color:G.amber,fontFamily:"monospace",fontWeight:700}}>{fromAisle}</span></div>
          <div style={{display:"grid",gap:10}}>
            {fromBins.map(loc=>(
              <button key={loc.id} onClick={()=>selectFromBin(loc.id)}
                style={{padding:"14px 16px",borderRadius:10,border:`2px solid ${BLUE}`,background:"#0a0d22",cursor:"pointer",textAlign:"left",display:"flex",alignItems:"center",justifyContent:"space-between"}}>
                <div>
                  <div style={{fontFamily:"monospace",fontWeight:800,fontSize:20,color:"#fff",marginBottom:4}}>{loc.id}</div>
                  {loc.contents.map((c,i)=><div key={i} style={{fontSize:13,color:"#aaa"}}><span style={{color:"#ddd",fontWeight:600}}>{c.description}</span> · <span style={{color:BLUE,fontWeight:700}}>{c.qty} ctns</span></div>)}
                </div>
                <div style={{color:BLUE,fontSize:22,marginLeft:10}}>›</div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={()=>{setStep("scan_from");setFromAisle("");}}>← SCAN DIFFERENT AISLE</button>
        </div>
      )}

      {step==="select_product" && fromLocObj && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:4}}>Select Product</div>
          <div style={{color:"#aaa",fontSize:14,marginBottom:14}}>Bin <span style={{color:G.amber,fontFamily:"monospace",fontWeight:700}}>{fromBin}</span></div>
          <div style={{display:"grid",gap:10}}>
            {fromLocObj.contents.map((c,i)=>(
              <button key={i} onClick={()=>{setFromProdIdx(i);setStep("dest_qty");}}
                style={{padding:"16px 18px",borderRadius:10,border:`2px solid ${BLUE}`,background:"#0a0d22",cursor:"pointer",textAlign:"left",display:"flex",justifyContent:"space-between",alignItems:"center"}}>
                <div>
                  <div style={{color:"#fff",fontWeight:800,fontSize:18}}>{c.description}</div>
                  <div style={{color:"#aaa",fontSize:13,marginTop:2}}>BB: {c.bestBefore||"—"}</div>
                </div>
                <div style={{textAlign:"right"}}>
                  <div style={{color:BLUE,fontWeight:800,fontSize:26,lineHeight:1}}>{c.qty}</div>
                  <div style={{color:"#aaa",fontSize:12}}>ctns</div>
                </div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={()=>setStep("select_from_bin")}>← BACK</button>
        </div>
      )}

      {/* Merged qty + destination on ONE screen */}
      {step==="dest_qty" && fromItem && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{background:"#0a0d22",border:`2px solid ${BLUE}`,borderRadius:10,padding:"12px 16px",marginBottom:16,display:"flex",justifyContent:"space-between",alignItems:"center"}}>
            <div>
              <div style={{fontSize:11,color:BLUE,fontWeight:700,letterSpacing:1}}>MOVING FROM {fromBin}</div>
              <div style={{color:"#fff",fontWeight:800,fontSize:17,marginTop:2}}>{fromItem.description}</div>
              <div style={{color:"#aaa",fontSize:12}}>BB: {fromItem.bestBefore||"—"} · {fromItem.qty} ctns avail.</div>
            </div>
            <button onClick={()=>doMoveAll()} style={{background:BLUE,border:"none",color:"#fff",borderRadius:8,padding:"10px 16px",cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",fontWeight:800,fontSize:14,whiteSpace:"nowrap"}}>MOVE ALL</button>
          </div>

          <label className="lbl-dark">QTY TO MOVE</label>
          <div style={{display:"flex",gap:8,marginBottom:8}}>
            {QUICK_QTYS.filter(q=>q<=fromItem.qty).map(q=>(
              <button key={q} onClick={()=>setMoveQty(String(q))}
                style={{flex:1,padding:"10px 0",background:moveQty===String(q)?BLUE:"#1e1e1e",border:`2px solid ${moveQty===String(q)?BLUE:"#333"}`,borderRadius:8,color:moveQty===String(q)?"#fff":"#888",fontWeight:800,fontSize:16,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif"}}>
                {q}
              </button>
            ))}
          </div>
          <input className="inp-dark" type="number" min="1" max={fromItem.qty} value={moveQty}
            onChange={e=>setMoveQty(e.target.value)} placeholder={`1–${fromItem.qty}`}
            style={{fontSize:24,fontWeight:800,textAlign:"center",minHeight:58}} />

          {/* TO destination inline */}
          {subMode==="aisle_to_floor" ? (
            <>
              <label className="lbl-dark" style={{marginTop:14}}>DESTINATION WAREHOUSE</label>
              <div style={{display:"grid",gap:8}}>
                {WAREHOUSES_CONST.map(wh=>(
                  <button key={wh} onClick={()=>setToWarehouse(wh)}
                    style={{padding:"12px 16px",background:toWarehouse===wh?"#1a1a0a":"#111",border:`2px solid ${toWarehouse===wh?G.amber:"#333"}`,borderRadius:8,cursor:"pointer",display:"flex",alignItems:"center",gap:10}}>
                    <div style={{width:10,height:10,borderRadius:"50%",background:toWarehouse===wh?G.amber:"#444"}}/>
                    <span style={{color:"#fff",fontWeight:700,fontSize:16}}>{wh.toUpperCase()}</span>
                  </button>
                ))}
              </div>
            </>
          ) : (
            <>
              <label className="lbl-dark" style={{marginTop:14}}>SCAN TO AISLE</label>
              <input ref={toRef} className="inp-dark" style={{textTransform:"uppercase",fontWeight:800,fontSize:24,letterSpacing:5,textAlign:"center"}}
                placeholder="K02" value={toAisle} onChange={e=>setToAisle(e.target.value.toUpperCase())}
                onKeyDown={e=>{if(e.key==="Enter"&&toAisle.trim()&&moveQty&&parseInt(moveQty)>0){if(!validateQty())return;lookupToAisle(toAisle);}}}
                autoComplete="off" />
            </>
          )}

          {error&&<ErrBox msg={error}/>}

          <button className="btn-xl-blue" style={{width:"100%",marginTop:14}} onClick={()=>{
            if(!validateQty()) return;
            if(subMode==="aisle_to_floor") {
              if(!toWarehouse){setError("Select a warehouse.");return;}
              setStep("confirm");
            } else {
              if(!toAisle.trim()){setError("Scan destination aisle.");return;}
              lookupToAisle(toAisle);
            }
          }}>
            {subMode==="aisle_to_floor" ? "CONFIRM MOVE →" : "CHECK TO AISLE →"}
          </button>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:10}} onClick={()=>setStep(fromLocObj?.contents.length>1?"select_product":"select_from_bin")}>← BACK</button>
        </div>
      )}

      {step==="select_to_bin" && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <SummaryBar product={fromItem} bestBefore={fromItem?.bestBefore} qty={moveQty} color={BLUE} label={`MOVING FROM ${fromBin}`}/>
          <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:4}}>Select Destination Bin</div>
          <div style={{color:"#aaa",fontSize:14,marginBottom:14}}>Aisle <span style={{color:G.amber,fontFamily:"monospace",fontWeight:700}}>{toAisle}</span></div>
          <div style={{display:"grid",gap:10}}>
            {toBinsFiltered.map(loc=>{
              const hasProd=loc.contents.find(c=>c.productCode===fromItem?.productCode);
              return(
                <button key={loc.id} onClick={()=>{setToBin(loc.id);setStep("confirm");}}
                  style={{padding:"14px 16px",borderRadius:10,border:`2px solid ${hasProd?G.amber:BLUE}`,background:hasProd?"#1a1800":"#0a0d22",cursor:"pointer",textAlign:"left",display:"flex",alignItems:"center",justifyContent:"space-between"}}>
                  <div>
                    <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:4}}>
                      <span style={{fontFamily:"monospace",fontWeight:800,fontSize:20,color:"#fff"}}>{loc.id}</span>
                      {hasProd&&<span style={{background:G.amberLight,color:G.amberDark,borderRadius:4,padding:"1px 7px",fontSize:11,fontWeight:700}}>HAS PRODUCT</span>}
                    </div>
                    {loc.contents.length===0?<div style={{fontSize:12,color:"#4a6aaa"}}>Empty</div>:loc.contents.map((c,i)=><div key={i} style={{fontSize:12,color:"#aaa"}}>{c.description} · {c.qty}</div>)}
                  </div>
                  <div style={{display:"flex",gap:4,flexShrink:0,marginLeft:10}}>
                    {Array.from({length:loc.maxProducts}).map((_,i)=>(
                      <div key={i} style={{width:20,height:20,borderRadius:4,background:i<loc.contents.length?G.amber:"#1e2a5e",border:`2px solid ${i<loc.contents.length?G.amberDark:"#2a3a8a"}`,display:"flex",alignItems:"center",justifyContent:"center",fontSize:9,color:"#fff"}}>
                        {i<loc.contents.length?"■":"○"}
                      </div>
                    ))}
                  </div>
                </button>
              );
            })}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={()=>{setStep("dest_qty");setToAisle("");setAisleToBins([]);}}>← SCAN DIFFERENT AISLE</button>
        </div>
      )}

      {step==="confirm" && fromItem && (
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{background:"#0a0d22",border:`2px solid ${BLUE}`,borderRadius:12,padding:"18px 20px",marginBottom:16}}>
            <div style={{fontSize:11,color:BLUE,fontWeight:700,letterSpacing:2,marginBottom:10}}>✓ CONFIRM MOVE</div>
            <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:14}}>{fromItem.description}</div>
            <div style={{display:"grid",gridTemplateColumns:"1fr auto 1fr",alignItems:"center",gap:10}}>
              <div style={{background:"#111",borderRadius:8,padding:"10px 12px",textAlign:"center"}}>
                <div style={{fontSize:11,color:"#666",letterSpacing:1,marginBottom:3}}>FROM</div>
                <div style={{color:G.red,fontWeight:800,fontSize:18,fontFamily:"monospace"}}>{fromBin}</div>
              </div>
              <div style={{color:BLUE,fontSize:26,fontWeight:800}}>⇄</div>
              <div style={{background:"#111",borderRadius:8,padding:"10px 12px",textAlign:"center"}}>
                <div style={{fontSize:11,color:"#666",letterSpacing:1,marginBottom:3}}>TO</div>
                <div style={{color:G.green,fontWeight:800,fontSize:18,fontFamily:"monospace"}}>
                  {subMode==="aisle_to_floor"?toWarehouse?.toUpperCase():toBin}
                </div>
                {subMode==="aisle_to_floor"&&<div style={{fontSize:11,color:"#888"}}>FLOOR</div>}
              </div>
            </div>
            <div style={{marginTop:12,textAlign:"center"}}>
              <div style={{color:"#fff",fontWeight:800,fontSize:34,lineHeight:1}}>{moveQty} <span style={{fontSize:15,color:"#aaa",fontWeight:400}}>ctns</span></div>
              <div style={{color:"#888",fontSize:12,marginTop:2}}>BB: {fromItem.bestBefore||"—"}</div>
            </div>
          </div>
          <button className="btn-xl-blue" style={{width:"100%",marginBottom:10}} onClick={confirmMove}>✓ CONFIRM MOVE</button>
          <button className="btn-outline-xl" style={{width:"100%"}} onClick={()=>setStep(subMode==="aisle_to_floor"?"dest_qty":"select_to_bin")}>← CHANGE DESTINATION</button>
          <button className="btn-ghost-sm" style={{width:"100%",marginTop:10}} onClick={onDone}>✕ CANCEL</button>
        </div>
      )}
    </div>
  );

  function doMoveAll() {
    setMoveQty(String(fromItem.qty));
  }
}

/* ── Move FROM Floor (optimised) ── */
function MoveFromFloor({ session, products, locations, setLocations, floorStock, setFloorStock, addLog, showToast, onDone, onBack }) {
  const BLUE = "#3a50cc";
  const [step, setStep]               = useState("pick_warehouse");
  const [warehouse, setWarehouse]     = useState(null);
  const [scanCode, setScanCode]       = useState("");
  const [matchedCode, setMatchedCode] = useState(null);
  const [selectedIdx, setSelectedIdx] = useState(null);
  const [moveQty, setMoveQty]         = useState("");
  const [toAisle, setToAisle]         = useState("");
  const [toBin, setToBin]             = useState(null);
  const [aisleToBins, setAisleToBins] = useState([]);
  const [error, setError]             = useState("");
  const scanRef = useRef(null); const toRef = useRef(null);

  useEffect(()=>{if(step==="scan_product")setTimeout(()=>scanRef.current?.focus(),80);},[step]);
  useEffect(()=>{if(step==="qty_dest")setTimeout(()=>toRef.current?.focus(),80);},[step]);

  const warehouseStock  = warehouse ? (floorStock[warehouse]||[]) : [];
  const matchedEntries  = matchedCode ? warehouseStock.map((e,i)=>({...e,_idx:i})).filter(e=>e.productCode===matchedCode) : [];
  const selectedEntry   = selectedIdx!==null ? warehouseStock[selectedIdx] : null;
  const toBinsFiltered  = aisleToBins.filter(l=>{const hp=l.contents.find(c=>c.productCode===selectedEntry?.productCode);return hp||l.contents.length<l.maxProducts;});

  const lookupProduct = (code) => {
    const inCat=findProduct(products, code);
    if(!inCat){setError(`Product not found. Try code or barcode.`);setScanCode("");return;}
    const onFloor=warehouseStock.filter(e=>e.productCode===c);
    if(!onFloor.length){setError(`No floor stock for "${inCat.description}" at ${warehouse}.`);setScanCode("");return;}
    setMatchedCode(c);setError("");
    if(onFloor.length===1){setSelectedIdx(warehouseStock.findIndex(e=>e.productCode===c));setStep("qty_dest");}
    else setStep("select_batch");
  };

  const lookupToAisle = (code) => {
    const c=code.trim().toUpperCase();
    const bins=Object.values(locations).filter(l=>l.id.startsWith(c)).sort((a,b)=>a.id.localeCompare(b.id));
    if(!bins.length){setError(`Aisle "${c}" not found.`);setToAisle("");return;}
    const free=bins.filter(l=>{const hp=l.contents.find(c2=>c2.productCode===selectedEntry?.productCode);return hp||l.contents.length<l.maxProducts;});
    if(!free.length){setError(`No space in "${c}".`);setToAisle("");return;}
    setAisleToBins(bins);setToAisle(c);setError("");
    if(free.length===1){setToBin(free[0].id);setStep("confirm");}
    else setStep("select_to_bin");
  };

  const validateQty=()=>{const n=parseInt(moveQty);if(!n||n<=0||n>selectedEntry.qty){setError(`Enter 1–${selectedEntry.qty}.`);return false;}return true;};

  const confirmMove=()=>{
    const n=parseInt(moveQty);
    const entry=selectedEntry;
    const newFloorQty=entry.qty-n;
    const newStock=newFloorQty===0?warehouseStock.filter((_,i)=>i!==selectedIdx):warehouseStock.map((e,i)=>i===selectedIdx?{...e,qty:newFloorQty}:e);
    const toLoc=locations[toBin];
    const hp=toLoc.contents.findIndex(c=>c.productCode===entry.productCode);
    const newToContents=hp>=0?toLoc.contents.map((c,i)=>i===hp?{...c,qty:c.qty+n}:c):[...toLoc.contents,{productCode:entry.productCode,description:entry.description,qty:n,bestBefore:entry.bestBefore,storedAt:new Date().toISOString()}];
    setFloorStock(prev=>({...prev,[warehouse]:newStock}));
    setLocations(prev=>({...prev,[toBin]:{...toLoc,contents:newToContents}}));
    addLog("MOVE",`${entry.description} · FLOOR/${warehouse} → ${toBin} · Qty:${n}`,session.username);
    showToast(`✓ Moved ${n} ctns: ${warehouse} floor → ${toBin}`);
    onDone();
  };

  const STEPS=["WAREHOUSE","SCAN PRODUCT","QTY + AISLE","CONFIRM"];
  const stepMap={pick_warehouse:0,scan_product:1,select_batch:1,qty_dest:2,select_to_bin:2,confirm:3};
  const stepIdx=stepMap[step]??0;

  return (
    <div>
      <StepBar steps={STEPS} current={stepIdx} color={BLUE}/>

      {step==="pick_warehouse"&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <SectionTitle icon="📦" color={BLUE} title="Select Warehouse" sub="Which floor are you moving stock from?"/>
          <div style={{display:"grid",gap:10}}>
            {WAREHOUSES_CONST.map(wh=>(
              <button key={wh} onClick={()=>{setWarehouse(wh);setStep("scan_product");}}
                style={{padding:"16px 18px",background:"#0a0d22",border:`2px solid ${BLUE}`,borderRadius:10,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"space-between"}}>
                <div style={{display:"flex",alignItems:"center",gap:12}}>
                  <div style={{width:10,height:10,borderRadius:"50%",background:BLUE}}/>
                  <div>
                    <div style={{color:"#fff",fontWeight:800,fontSize:18}}>{wh.toUpperCase()}</div>
                    <div style={{color:"#555",fontSize:12}}>{(floorStock[wh]||[]).length} entries on floor</div>
                  </div>
                </div>
                <span style={{color:BLUE,fontSize:20}}>›</span>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={onBack}>← BACK</button>
        </div>
      )}

      {step==="scan_product"&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:14}}>
            <span style={{background:G.amberLight,color:G.amberDark,border:`1px solid ${G.amber}`,borderRadius:6,padding:"4px 12px",fontSize:13,fontWeight:700}}>📦 {warehouse?.toUpperCase()} FLOOR</span>
            <span style={{color:"#555",fontSize:13}}>{warehouseStock.length} entries</span>
          </div>
          {warehouseStock.length===0?(
            <div style={{background:"#111",border:"2px solid #333",borderRadius:12,padding:"32px 20px",textAlign:"center"}}>
              <div style={{fontSize:40,marginBottom:10}}>📭</div>
              <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:8}}>No Floor Stock</div>
              <div style={{color:"#888",fontSize:14,marginBottom:20}}>Nothing on floor at <span style={{color:G.amber,fontWeight:700}}>{warehouse}</span>.</div>
              <button className="btn-outline-xl" style={{width:"100%"}} onClick={()=>setStep("pick_warehouse")}>← CHANGE WAREHOUSE</button>
            </div>
          ):(
            <>
              <SectionTitle icon="⇄" color={BLUE} title="Scan or Search Product" sub={`Floor at ${warehouse}`}/>
              <input ref={scanRef} className="inp-dark" style={{fontWeight:800,fontSize:22,letterSpacing:2,textAlign:"center"}}
                placeholder="SCAN BARCODE" value={scanCode} onChange={e=>setScanCode(e.target.value)}
                onKeyDown={e=>{if(e.key==="Enter"&&scanCode.trim())lookupProduct(scanCode);}} autoComplete="off"/>
              <div style={{display:"flex",alignItems:"center",gap:10,margin:"8px 0"}}>
                <div style={{flex:1,height:1,background:"#2a2a2a"}}/><span style={{color:"#555",fontSize:12,fontWeight:700}}>OR</span><div style={{flex:1,height:1,background:"#2a2a2a"}}/>
              </div>
              <ProductCodeSearch products={products} onSelect={p=>{setScanCode(p.code);lookupProduct(p.code);}}/>
              {error&&<ErrBox msg={error}/>}
              <button className="btn-xl-blue" style={{width:"100%",marginTop:10}} onClick={()=>scanCode.trim()&&lookupProduct(scanCode)}>FIND →</button>
            </>
          )}
          <button className="btn-outline-xl" style={{width:"100%",marginTop:10}} onClick={()=>setStep("pick_warehouse")}>← CHANGE WAREHOUSE</button>
        </div>
      )}

      {step==="select_batch"&&matchedCode&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <ProductFoundCard product={products.find(p=>p.code===matchedCode)||{code:matchedCode,description:matchedEntries[0]?.description}}/>
          <div style={{color:"#fff",fontWeight:800,fontSize:18,margin:"14px 0 6px"}}>Select Batch</div>
          <div style={{display:"grid",gap:10}}>
            {matchedEntries.map(e=>(
              <button key={e._idx} onClick={()=>{setSelectedIdx(e._idx);setStep("qty_dest");}}
                style={{padding:"14px 16px",borderRadius:10,border:`2px solid ${BLUE}`,background:"#0a0d22",cursor:"pointer",textAlign:"left",display:"flex",justifyContent:"space-between",alignItems:"center"}}>
                <div>
                  <div style={{fontSize:11,color:BLUE,fontWeight:700,letterSpacing:1}}>BEST BEFORE</div>
                  <div style={{color:"#fff",fontWeight:800,fontSize:18,marginTop:2}}>{e.bestBefore||"No date"}</div>
                </div>
                <div style={{textAlign:"right"}}>
                  <div style={{color:BLUE,fontWeight:800,fontSize:26}}>{e.qty}</div>
                  <div style={{color:"#aaa",fontSize:12}}>ctns</div>
                </div>
              </button>
            ))}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={()=>{setStep("scan_product");setMatchedCode(null);setScanCode("");}}>← BACK</button>
        </div>
      )}

      {/* Merged qty + TO aisle on one screen */}
      {step==="qty_dest"&&selectedEntry&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{background:"#0a0d22",border:`2px solid ${BLUE}`,borderRadius:10,padding:"12px 16px",marginBottom:14,display:"flex",justifyContent:"space-between",alignItems:"center"}}>
            <div>
              <div style={{fontSize:11,color:BLUE,fontWeight:700}}>FROM {warehouse?.toUpperCase()} FLOOR</div>
              <div style={{color:"#fff",fontWeight:800,fontSize:17,marginTop:2}}>{selectedEntry.description}</div>
              <div style={{color:"#aaa",fontSize:12}}>BB: {selectedEntry.bestBefore||"—"} · {selectedEntry.qty} ctns avail.</div>
            </div>
            <button onClick={()=>{setMoveQty(String(selectedEntry.qty));}} style={{background:BLUE,border:"none",color:"#fff",borderRadius:8,padding:"10px 14px",cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",fontWeight:800,fontSize:14,whiteSpace:"nowrap"}}>MOVE ALL</button>
          </div>

          <label className="lbl-dark">QTY TO MOVE</label>
          <div style={{display:"flex",gap:8,marginBottom:8}}>
            {QUICK_QTYS.filter(q=>q<=selectedEntry.qty).map(q=>(
              <button key={q} onClick={()=>setMoveQty(String(q))}
                style={{flex:1,padding:"10px 0",background:moveQty===String(q)?BLUE:"#1e1e1e",border:`2px solid ${moveQty===String(q)?BLUE:"#333"}`,borderRadius:8,color:moveQty===String(q)?"#fff":"#888",fontWeight:800,fontSize:16,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif"}}>
                {q}
              </button>
            ))}
          </div>
          <input className="inp-dark" type="number" min="1" max={selectedEntry.qty} value={moveQty}
            onChange={e=>setMoveQty(e.target.value)} placeholder={`1–${selectedEntry.qty}`}
            style={{fontSize:24,fontWeight:800,textAlign:"center",minHeight:58}}/>

          <label className="lbl-dark" style={{marginTop:14}}>SCAN TO AISLE</label>
          <input ref={toRef} className="inp-dark" style={{textTransform:"uppercase",fontWeight:800,fontSize:24,letterSpacing:5,textAlign:"center"}}
            placeholder="J01" value={toAisle} onChange={e=>setToAisle(e.target.value.toUpperCase())}
            onKeyDown={e=>{if(e.key==="Enter"&&toAisle.trim()&&moveQty&&parseInt(moveQty)>0){if(!validateQty())return;lookupToAisle(toAisle);}}}
            autoComplete="off"/>
          {error&&<ErrBox msg={error}/>}
          <button className="btn-xl-blue" style={{width:"100%",marginTop:14}} onClick={()=>{if(!validateQty())return;if(!toAisle.trim()){setError("Scan destination aisle.");return;}lookupToAisle(toAisle);}}>CHECK AISLE →</button>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:10}} onClick={()=>setStep(matchedEntries.length>1?"select_batch":"scan_product")}>← BACK</button>
        </div>
      )}

      {step==="select_to_bin"&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <SummaryBar product={selectedEntry} bestBefore={selectedEntry?.bestBefore} qty={moveQty} color={BLUE} label={`FROM ${warehouse} FLOOR`}/>
          <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:4}}>Select Destination Bin</div>
          <div style={{color:"#aaa",fontSize:14,marginBottom:14}}>Aisle <span style={{color:G.amber,fontFamily:"monospace",fontWeight:700}}>{toAisle}</span></div>
          <div style={{display:"grid",gap:10}}>
            {toBinsFiltered.map(loc=>{
              const hasProd=loc.contents.find(c=>c.productCode===selectedEntry?.productCode);
              return(
                <button key={loc.id} onClick={()=>{setToBin(loc.id);setStep("confirm");}}
                  style={{padding:"14px 16px",borderRadius:10,border:`2px solid ${hasProd?G.amber:BLUE}`,background:hasProd?"#1a1800":"#0a0d22",cursor:"pointer",textAlign:"left",display:"flex",alignItems:"center",justifyContent:"space-between"}}>
                  <div>
                    <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:4}}>
                      <span style={{fontFamily:"monospace",fontWeight:800,fontSize:20,color:"#fff"}}>{loc.id}</span>
                      {hasProd&&<span style={{background:G.amberLight,color:G.amberDark,borderRadius:4,padding:"1px 7px",fontSize:11,fontWeight:700}}>HAS PRODUCT</span>}
                    </div>
                    {loc.contents.length===0?<div style={{fontSize:12,color:"#4a6aaa"}}>Empty</div>:loc.contents.map((c,i)=><div key={i} style={{fontSize:12,color:"#aaa"}}>{c.description} · {c.qty}</div>)}
                  </div>
                  <div style={{display:"flex",gap:4,flexShrink:0,marginLeft:10}}>
                    {Array.from({length:loc.maxProducts}).map((_,i)=>(
                      <div key={i} style={{width:20,height:20,borderRadius:4,background:i<loc.contents.length?G.amber:"#1e2a5e",border:`2px solid ${i<loc.contents.length?G.amberDark:"#2a3a8a"}`,display:"flex",alignItems:"center",justifyContent:"center",fontSize:9,color:"#fff"}}>
                        {i<loc.contents.length?"■":"○"}
                      </div>
                    ))}
                  </div>
                </button>
              );
            })}
          </div>
          <button className="btn-outline-xl" style={{width:"100%",marginTop:14}} onClick={()=>{setStep("qty_dest");setToAisle("");setAisleToBins([]);}}>← SCAN DIFFERENT AISLE</button>
        </div>
      )}

      {step==="confirm"&&selectedEntry&&(
        <div style={{animation:"fadeUp .2s ease"}}>
          <div style={{background:"#0a0d22",border:`2px solid ${BLUE}`,borderRadius:12,padding:"18px 20px",marginBottom:16}}>
            <div style={{fontSize:11,color:BLUE,fontWeight:700,letterSpacing:2,marginBottom:10}}>✓ CONFIRM MOVE</div>
            <div style={{color:"#fff",fontWeight:800,fontSize:20,marginBottom:14}}>{selectedEntry.description}</div>
            <div style={{display:"grid",gridTemplateColumns:"1fr auto 1fr",alignItems:"center",gap:10}}>
              <div style={{background:"#111",borderRadius:8,padding:"10px 12px",textAlign:"center"}}>
                <div style={{fontSize:11,color:"#666",letterSpacing:1,marginBottom:3}}>FROM</div>
                <div style={{color:G.amber,fontWeight:800,fontSize:16}}>{warehouse?.toUpperCase()}</div>
                <div style={{fontSize:11,color:"#888"}}>FLOOR</div>
              </div>
              <div style={{color:BLUE,fontSize:26,fontWeight:800}}>⇄</div>
              <div style={{background:"#111",borderRadius:8,padding:"10px 12px",textAlign:"center"}}>
                <div style={{fontSize:11,color:"#666",letterSpacing:1,marginBottom:3}}>TO</div>
                <div style={{color:G.green,fontWeight:800,fontSize:18,fontFamily:"monospace"}}>{toBin}</div>
              </div>
            </div>
            <div style={{marginTop:12,textAlign:"center"}}>
              <div style={{color:"#fff",fontWeight:800,fontSize:34,lineHeight:1}}>{moveQty} <span style={{fontSize:15,color:"#aaa",fontWeight:400}}>ctns</span></div>
              <div style={{color:"#888",fontSize:12,marginTop:2}}>BB: {selectedEntry.bestBefore||"—"}</div>
            </div>
          </div>
          <button className="btn-xl-blue" style={{width:"100%",marginBottom:10}} onClick={confirmMove}>✓ CONFIRM MOVE</button>
          <button className="btn-outline-xl" style={{width:"100%"}} onClick={()=>setStep("select_to_bin")}>← CHANGE BIN</button>
          <button className="btn-ghost-sm" style={{width:"100%",marginTop:10}} onClick={onDone}>✕ CANCEL</button>
        </div>
      )}
    </div>
  );
}



/* ═══════════════════════════════════════════════════════════════════════════
   SHARED PICKER UI COMPONENTS
══════════════════════════════════════════════════════════════════════════ */
/* Inline warehouse picker used on the scan_aisle step */
function FloorWarehousePicker({ onSelect }) {
  const [open, setOpen] = useState(false);
  return (
    <div style={{ borderRadius: 10, overflow: "hidden", border: `2px solid ${open ? G.amber : "#3a3a2a"}`, transition: "border .2s" }}>
      <button onClick={() => setOpen(o => !o)}
        style={{ width: "100%", padding: "16px 18px", background: open ? "#1a1a0a" : "#141410", border: "none", cursor: "pointer", display: "flex", alignItems: "center", gap: 14 }}>
        <div style={{ width: 44, height: 44, borderRadius: 10, background: "#2a2a10", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22, flexShrink: 0 }}>📦</div>
        <div style={{ textAlign: "left", flex: 1 }}>
          <div style={{ color: "#fff", fontWeight: 800, fontSize: 18 }}>STORE ON FLOOR</div>
          <div style={{ color: "#888", fontSize: 13, marginTop: 1 }}>No aisle space? Store at a warehouse floor</div>
        </div>
        <div style={{ color: G.amber, fontSize: 22, transform: open ? "rotate(90deg)" : "none", transition: "transform .2s" }}>›</div>
      </button>
      {open && (
        <div style={{ borderTop: `1px solid #2a2a10`, padding: "10px 14px 14px", background: "#111", animation: "fadeUp .15s ease" }}>
          <div style={{ fontSize: 11, color: "#666", letterSpacing: 1, fontWeight: 700, marginBottom: 10 }}>SELECT WAREHOUSE:</div>
          <div style={{ display: "grid", gap: 8 }}>
            {WAREHOUSES_CONST.map(wh => (
              <button key={wh} onClick={() => onSelect(wh)}
                style={{ padding: "14px 18px", background: "#1a1a0a", border: `2px solid ${G.amber}`, borderRadius: 8, cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "space-between" }}>
                <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                  <div style={{ width: 10, height: 10, borderRadius: "50%", background: G.amber, flexShrink: 0 }} />
                  <span style={{ color: "#fff", fontWeight: 800, fontSize: 17 }}>{wh.toUpperCase()}</span>
                </div>
                <span style={{ color: G.amber, fontSize: 18 }}>›</span>
              </button>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}

/* Manual product code search — used across Add/Remove/Move when barcode unavailable */
function ProductCodeSearch({ products, onSelect }) {
  const [q, setQ] = useState("");
  const [open, setOpen] = useState(false);
  const results = q.trim()
    ? products.filter(p =>
        p.code.toLowerCase().includes(q.toLowerCase()) ||
        p.description.toLowerCase().includes(q.toLowerCase())
      ).slice(0, 6)
    : [];

  return (
    <div>
      <div style={{ position:"relative" }}>
        <input className="inp-dark"
          style={{ fontSize:16, letterSpacing:1 }}
          placeholder="Type product code (e.g. P001) or name…"
          value={q}
          onChange={e => { setQ(e.target.value); setOpen(true); }}
          onFocus={() => setOpen(true)}
          autoComplete="off"
        />
        {q && <button onClick={()=>{setQ("");setOpen(false);}} style={{ position:"absolute", right:12, top:"50%", transform:"translateY(-50%)", background:"none", border:"none", color:"#666", fontSize:18, cursor:"pointer" }}>✕</button>}
      </div>
      {open && results.length > 0 && (
        <div style={{ background:"#1e1e1e", border:"1px solid #333", borderRadius:8, marginTop:4, overflow:"hidden" }}>
          {results.map(p => (
            <div key={p.code} onClick={()=>{ onSelect(p); setQ(""); setOpen(false); }}
              style={{ display:"flex", alignItems:"center", justifyContent:"space-between", padding:"12px 14px", borderBottom:"1px solid #2a2a2a", cursor:"pointer" }}>
              <div>
                <div style={{ color:"#fff", fontWeight:700, fontSize:15 }}>{p.description}</div>
                <div style={{ color:"#888", fontSize:12, fontFamily:"monospace", marginTop:2 }}>Code: {p.code}{p.barcode?` · Barcode: ${p.barcode}`:""}</div>
              </div>
              <div style={{ color:G.amber, fontSize:18 }}>›</div>
            </div>
          ))}
        </div>
      )}
      {open && q.trim() && results.length === 0 && (
        <div style={{ background:"#1e1e1e", border:"1px solid #333", borderRadius:8, marginTop:4, padding:"12px 14px", color:"#666", fontSize:14 }}>No products match "{q}"</div>
      )}
    </div>
  );
}

function StepBar({ steps, current, color }) {
  return (
    <div style={{ display: "flex", marginBottom: 24, gap: 3 }}>
      {steps.map((label, i) => (
        <div key={i} style={{ flex: 1, textAlign: "center" }}>
          <div style={{ height: 4, borderRadius: 2, background: i <= current ? color : "#2a2a2a", marginBottom: 4, transition: "background .3s" }} />
          <div style={{ fontSize: 10, fontWeight: 700, color: i === current ? color : i < current ? "#4a7a4a" : "#444", letterSpacing: .3 }}>{i < current ? "✓" : label}</div>
        </div>
      ))}
    </div>
  );
}

function SectionTitle({ icon, color, title, sub }) {
  return (
    <div style={{ marginBottom: 20 }}>
      <div style={{ color, fontWeight: 800, fontSize: 24, display: "flex", alignItems: "center", gap: 8 }}><span>{icon}</span>{title}</div>
      {sub && <div style={{ color: "#888", fontSize: 14, marginTop: 4 }}>{sub}</div>}
    </div>
  );
}

function ProductFoundCard({ product }) {
  return (
    <div style={{ background: "#222", border: `2px solid ${G.amber}`, borderRadius: 10, padding: "16px 18px", marginBottom: 4 }}>
      <div style={{ fontSize: 11, color: G.amber, letterSpacing: 2, fontWeight: 700, marginBottom: 4 }}>✓ PRODUCT FOUND</div>
      <div style={{ fontWeight: 800, fontSize: 22, color: "#fff" }}>{product.description}</div>
      <div style={{ fontFamily: "monospace", color: G.amber, fontSize: 14, marginTop: 4 }}>{product.code}</div>
      {product.category && <div style={{ color: "#888", fontSize: 13, marginTop: 2 }}>{product.category}</div>}
    </div>
  );
}

function SummaryBar({ product, bestBefore, qty, color, label }) {
  return (
    <div style={{ background: "#1e1e1e", borderRadius: 8, padding: "12px 16px", marginBottom: 20, display: "flex", gap: 16, alignItems: "center", border: "1px solid #2a2a2a" }}>
      <div style={{ flex: 1 }}>
        <div style={{ color, fontSize: 11, fontWeight: 700, letterSpacing: 1 }}>{label}</div>
        <div style={{ color: "#fff", fontWeight: 800, fontSize: 15 }}>{product?.description}</div>
        {bestBefore && <div style={{ color: "#aaa", fontSize: 12, marginTop: 1 }}>BB: {bestBefore}</div>}
      </div>
      {qty && <div style={{ textAlign: "right" }}><div style={{ color: "#aaa", fontSize: 11 }}>QTY</div><div style={{ color: "#fff", fontWeight: 800, fontSize: 24 }}>{qty}</div></div>}
    </div>
  );
}
/* ─── SHARED COMPONENTS ─────────────────────────────────────────────────── */
function ErrBox({ msg }) {
  return <div style={{ background: "#fef2f2", border: "1px solid #fca5a5", borderRadius: 6, padding: "10px 14px", marginTop: 10, color: G.red, fontWeight: 700, fontSize: 14 }}>⚠ {msg}</div>;
}
function Toast({ toast, dark }) {
  const bg = toast.type === "ok" ? G.green : toast.type === "err" ? G.red : G.amber;
  return <div style={{ position: "fixed", bottom: 24, left: "50%", transform: "translateX(-50%)", background: bg, color: "#fff", padding: "14px 22px", borderRadius: 8, fontSize: 15, fontWeight: 700, boxShadow: "0 4px 20px rgba(0,0,0,.3)", zIndex: 300, whiteSpace: "nowrap", animation: "fadeUp .2s ease" }}>{toast.msg}</div>;
}
function BarcodeIcon() {
  return (
    <div style={{ display: "flex", alignItems: "center", gap: 2 }}>
      {[12, 8, 16, 6, 12].map((h, i) => <div key={i} style={{ width: 3, height: h, background: G.amber, borderRadius: 1 }} />)}
    </div>
  );
}

/* ─── GLOBAL CSS ────────────────────────────────────────────────────────── */
const css = `
@import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;500;600;700;800&family=Barlow:wght@400;500;600&display=swap');
*{box-sizing:border-box;margin:0;padding:0;}
::-webkit-scrollbar{width:5px;height:5px;}
::-webkit-scrollbar-thumb{background:#e8a000;border-radius:3px;}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
@keyframes fadeUp{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}

.lbl{font-size:12px;font-weight:700;color:#777;letter-spacing:1px;text-transform:uppercase;display:block;margin-bottom:5px;}
.lbl-dark{font-size:12px;font-weight:700;color:#aaa;letter-spacing:1px;text-transform:uppercase;display:block;margin-bottom:6px;margin-top:4px;}

.inp{background:#f8f8f6;border:2px solid #e0e0dc;color:#1a1a1a;border-radius:6px;width:100%;outline:none;font-size:15px;padding:11px 14px;min-height:46px;font-family:'Barlow Condensed',sans-serif;transition:border .15s;}
.inp:focus{border-color:#E8A000;background:#fff;}

.inp-dark{background:#1e1e1e;border:2px solid #333;color:#fff;border-radius:8px;width:100%;outline:none;font-size:17px;padding:14px 16px;min-height:54px;font-family:'Barlow Condensed',sans-serif;transition:border .15s;}
.inp-dark:focus{border-color:#E8A000;}
.inp-dark::placeholder{color:#555;}

.sel{background:#f8f8f6;border:2px solid #e0e0dc;color:#1a1a1a;border-radius:6px;outline:none;cursor:pointer;font-size:15px;padding:10px 12px;min-height:46px;font-family:'Barlow Condensed',sans-serif;}
.sel:focus{border-color:#E8A000;}

.btn-amber{background:#E8A000;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;letter-spacing:.5px;border-radius:6px;padding:11px 22px;font-size:15px;min-height:44px;text-transform:uppercase;transition:all .15s;}
.btn-amber:hover{background:#f0b020;}
.btn-amber:active{transform:scale(.97);}

.btn-amber-xl{background:#E8A000;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;letter-spacing:.5px;border-radius:8px;padding:17px 24px;font-size:18px;min-height:58px;text-transform:uppercase;transition:all .15s;}
.btn-amber-xl:hover{background:#f0b020;}
.btn-amber-xl:active{transform:scale(.97);}

.btn-green-xl{background:#16a34a;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;border-radius:8px;padding:17px 24px;font-size:18px;min-height:58px;text-transform:uppercase;transition:all .15s;}
.btn-green-xl:hover{background:#15803d;}
.btn-green-xl:active{transform:scale(.97);}
.btn-red-xl{background:#dc2626;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;border-radius:8px;padding:17px 24px;font-size:18px;min-height:58px;text-transform:uppercase;transition:all .15s;}
.btn-red-xl:hover{background:#b91c1c;}
.btn-red-xl:active{transform:scale(.97);}
.btn-xl-blue{background:#3a50cc;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;border-radius:8px;padding:17px 24px;font-size:18px;min-height:58px;text-transform:uppercase;transition:all .15s;}
.btn-xl-blue:hover{background:#4a62e0;}
.btn-xl-blue:active{transform:scale(.97);}

.btn-outline-xl{background:transparent;color:#888;border:2px solid #444;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;border-radius:8px;padding:15px 24px;font-size:17px;min-height:56px;text-transform:uppercase;transition:all .15s;}
.btn-outline-xl:hover{border-color:#888;color:#ccc;}

.btn-amber-sm{background:#E8A000;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;border-radius:5px;padding:7px 14px;font-size:13px;min-height:34px;text-transform:uppercase;}
.btn-ghost-sm{background:transparent;color:#888;border:1px solid #ccc;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;border-radius:5px;padding:7px 14px;font-size:13px;min-height:34px;text-transform:uppercase;transition:all .15s;}
.btn-ghost-sm:hover{border-color:#E8A000;color:#E8A000;}
.btn-green-sm{background:#16a34a;color:#fff;border:none;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;border-radius:5px;padding:7px 14px;font-size:13px;min-height:34px;text-transform:uppercase;}
.btn-red-sm{background:transparent;color:#dc2626;border:1px solid #dc2626;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;border-radius:5px;padding:7px 14px;font-size:13px;min-height:34px;text-transform:uppercase;}
.btn-red-sm:hover{background:#dc2626;color:#fff;}
`;
