import React, { useState, useEffect, useCallback } from "react";
import {
  School, LogOut, Plus, Pencil, Trash2, X, Save, Users, GraduationCap,
  Wallet, ClipboardCheck, Award, Smile, ShieldCheck, MapPin, Check, Loader2
} from "lucide-react";

const STORAGE_KEY = "nlw-school-data";
const ADMIN_ID = "admin";
const ADMIN_PASS = "admin123";
const CLASSES = ["Play Group", "Nursery", "LKG", "UKG"];
const AVATARS = ["🦁", "🐻", "🐰", "🦊", "🐼", "🐸", "🦋", "🐧"];
const THEMES = {
  coral: { name: "Coral", bg: "#FFE3DE", accent: "#FF6B6B" },
  teal: { name: "Teal", bg: "#DEF7F4", accent: "#2FB6A8" },
  sun: { name: "Sunshine", bg: "#FFF3D6", accent: "#F2A93B" },
  grape: { name: "Grape", bg: "#EDE3FA", accent: "#8B5CF6" },
  leaf: { name: "Leaf", bg: "#E1F5E6", accent: "#3FA65C" },
};

const uid = () => Math.random().toString(36).slice(2, 9);

const seedData = () => ({
  students: [
    {
      id: "101", name: "Aarav Sharma", class: "Nursery", password: "3",
      avatar: "🦁", theme: "coral",
      attendance: { present: 20, total: 24 },
      fees: { total: 6000, paid: 4000 },
      result: { subjects: [{ name: "English Alphabets", marks: 42, max: 50 }, { name: "Numbers", marks: 38, max: 50 }, { name: "Rhymes & Music", marks: 45, max: 50 }], remarks: "Aarav is doing wonderfully well!" },
    },
    {
      id: "102", name: "Ishita Verma", class: "LKG", password: "1",
      avatar: "🐰", theme: "grape",
      attendance: { present: 22, total: 24 },
      fees: { total: 7000, paid: 7000 },
      result: { subjects: [{ name: "Reading", marks: 47, max: 50 }, { name: "Drawing", marks: 44, max: 50 }, { name: "GK", marks: 40, max: 50 }], remarks: "Excellent progress this term." },
    },
  ],
  teachers: [
    { id: "T1", name: "Priya Mehta", password: "teach123", assignedClass: "All" },
  ],
});

function useSchoolData() {
  const [data, setData] = useState(null);
  const [status, setStatus] = useState("loading");

  useEffect(() => {
    (async () => {
      try {
        const res = await window.storage.get(STORAGE_KEY, true);
        if (res && res.value) {
          setData(JSON.parse(res.value));
        } else {
          const seed = seedData();
          await window.storage.set(STORAGE_KEY, JSON.stringify(seed), true);
          setData(seed);
        }
        setStatus("ready");
      } catch (e) {
        const seed = seedData();
        try {
          await window.storage.set(STORAGE_KEY, JSON.stringify(seed), true);
        } catch (_) {}
        setData(seed);
        setStatus("ready");
      }
    })();
  }, []);

  const persist = useCallback(async (next) => {
    setData(next);
    try {
      await window.storage.set(STORAGE_KEY, JSON.stringify(next), true);
    } catch (e) {
      console.error("Save failed", e);
    }
  }, []);

  return { data, status, persist };
}

function pct(a, b) {
  if (!b) return 0;
  return Math.round((a / b) * 100);
}

function ProgressBar({ value, color }) {
  return (
    <div style={{ background: "#00000012" }} className="w-full h-2.5 rounded-full overflow-hidden">
      <div
        className="h-full rounded-full transition-all"
        style={{ width: `${Math.min(100, Math.max(0, value))}%`, background: color }}
      />
    </div>
  );
}

function Header({ session, onLogout }) {
  return (
    <div className="flex items-center justify-between px-5 md:px-8 py-4 bg-white/70 backdrop-blur border-b border-[#00000010]">
      <div className="flex items-center gap-3">
        <div className="w-10 h-10 rounded-2xl flex items-center justify-center text-xl" style={{ background: "#FFD166" }}>🎓</div>
        <div>
          <div className="font-bold text-[#2D2A4A] leading-tight" style={{ fontFamily: "'Fredoka', sans-serif" }}>New Learning Wing</div>
          <div className="text-xs text-[#2D2A4A99] leading-tight">Pre School</div>
        </div>
      </div>
      <div className="flex items-center gap-4">
        <div className="text-right hidden sm:block">
          <div className="text-sm font-semibold text-[#2D2A4A]">{session.name}</div>
          <div className="text-xs text-[#2D2A4A99] capitalize">{session.role}</div>
        </div>
        <button
          onClick={onLogout}
          className="flex items-center gap-1.5 px-3 py-2 rounded-xl bg-[#2D2A4A] text-white text-sm font-medium hover:opacity-90 active:scale-95 transition"
        >
          <LogOut size={15} /> Logout
        </button>
      </div>
    </div>
  );
}

function LoginScreen({ data, onLogin }) {
  const [role, setRole] = useState("admin");
  const [id, setId] = useState("");
  const [pass, setPass] = useState("");
  const [error, setError] = useState("");

  const roles = [
    { key: "admin", label: "Admin", icon: ShieldCheck },
    { key: "teacher", label: "Teacher", icon: Users },
    { key: "student", label: "Student", icon: Smile },
  ];

  function handleSubmit(e) {
    e.preventDefault();
    setError("");
    if (role === "admin") {
      if (id === ADMIN_ID && pass === ADMIN_PASS) {
        onLogin({ role: "admin", id: ADMIN_ID, name: "Administrator" });
      } else {
        setError("Galat admin ID ya password.");
      }
      return;
    }
    if (role === "teacher") {
      const t = data.teachers.find((t) => t.id.toLowerCase() === id.toLowerCase() && t.password === pass);
      if (t) onLogin({ role: "teacher", id: t.id, name: t.name, assignedClass: t.assignedClass });
      else setError("Teacher ID ya password galat hai.");
      return;
    }
    const s = data.students.find((s) => s.id === id.trim() && s.password === pass);
    if (s) onLogin({ role: "student", id: s.id, name: s.name });
    else setError("Roll number ya password galat hai.");
  }

  return (
    <div className="min-h-screen w-full flex items-center justify-center p-4 relative overflow-hidden" style={{ background: "linear-gradient(160deg, #FFF9F0 0%, #FFF0DE 100%)" }}>
      <div className="absolute -top-16 -left-16 w-64 h-64 rounded-full opacity-40" style={{ background: "#FFD166" }} />
      <div className="absolute -bottom-20 -right-10 w-80 h-80 rounded-full opacity-30" style={{ background: "#4ECDC4" }} />
      <div className="absolute top-1/3 right-10 w-24 h-24 rounded-full opacity-20 hidden md:block" style={{ background: "#FF6B6B" }} />

      <div className="relative w-full max-w-md">
        <div className="text-center mb-6">
          <div className="w-16 h-16 mx-auto rounded-3xl flex items-center justify-center text-3xl shadow-md mb-3" style={{ background: "#FF6B6B" }}>🎓</div>
          <h1 className="text-2xl font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>New Learning Wing Pre School</h1>
          <div className="flex items-center justify-center gap-1 text-sm text-[#2D2A4A99] mt-1">
            <MapPin size={13} /> Nindura, Barabanki
          </div>
        </div>

        <div className="bg-white rounded-3xl shadow-xl p-6 border border-[#00000008]">
          <div className="grid grid-cols-3 gap-2 mb-5">
            {roles.map((r) => {
              const Icon = r.icon;
              const active = role === r.key;
              return (
                <button
                  key={r.key}
                  onClick={() => { setRole(r.key); setError(""); setId(""); setPass(""); }}
                  className="flex flex-col items-center gap-1 py-3 rounded-2xl text-xs font-semibold transition"
                  style={{
                    background: active ? "#2D2A4A" : "#F5F3EE",
                    color: active ? "#fff" : "#2D2A4A",
                  }}
                >
                  <Icon size={18} />
                  {r.label}
                </button>
              );
            })}
          </div>

          <form onSubmit={handleSubmit} className="space-y-3">
            <div>
              <label className="text-xs font-semibold text-[#2D2A4A99] mb-1 block">
                {role === "admin" ? "Admin ID" : role === "teacher" ? "Teacher ID" : "Roll Number"}
              </label>
              <input
                value={id}
                onChange={(e) => setId(e.target.value)}
                className="w-full px-4 py-2.5 rounded-xl border border-[#00000015] focus:outline-none focus:ring-2 focus:ring-[#FF6B6B] text-[#2D2A4A]"
                placeholder={role === "student" ? "e.g. 101" : "ID daaliye"}
                required
              />
            </div>
            <div>
              <label className="text-xs font-semibold text-[#2D2A4A99] mb-1 block">Password</label>
              {role === "student" ? (
                <select
                  value={pass}
                  onChange={(e) => setPass(e.target.value)}
                  className="w-full px-4 py-2.5 rounded-xl border border-[#00000015] focus:outline-none focus:ring-2 focus:ring-[#FF6B6B] text-[#2D2A4A] bg-white"
                  required
                >
                  <option value="">Chuniye (1-5)</option>
                  {[1, 2, 3, 4, 5].map((n) => <option key={n} value={n}>{n}</option>)}
                </select>
              ) : (
                <input
                  type="password"
                  value={pass}
                  onChange={(e) => setPass(e.target.value)}
                  className="w-full px-4 py-2.5 rounded-xl border border-[#00000015] focus:outline-none focus:ring-2 focus:ring-[#FF6B6B] text-[#2D2A4A]"
                  placeholder="Password"
                  required
                />
              )}
            </div>
            {error && <div className="text-sm text-[#FF6B6B] font-medium">{error}</div>}
            <button
              type="submit"
              className="w-full py-3 rounded-xl font-bold text-white shadow-md active:scale-[0.98] transition"
              style={{ background: "#FF6B6B", fontFamily: "'Fredoka', sans-serif" }}
            >
              Login
            </button>
          </form>
        </div>
        <p className="text-center text-xs text-[#2D2A4A80] mt-4">Admin default &mdash; ID: admin, Password: admin123</p>
      </div>
    </div>
  );
}

function TabButton({ active, onClick, icon: Icon, label }) {
  return (
    <button
      onClick={onClick}
      className="flex items-center gap-2 px-4 py-2.5 rounded-xl text-sm font-semibold whitespace-nowrap transition"
      style={{ background: active ? "#2D2A4A" : "transparent", color: active ? "#fff" : "#2D2A4A" }}
    >
      <Icon size={16} /> {label}
    </button>
  );
}

function StatCard({ label, value, icon: Icon, color }) {
  return (
    <div className="bg-white rounded-2xl p-4 shadow-sm border border-[#00000008] flex items-center gap-3">
      <div className="w-10 h-10 rounded-xl flex items-center justify-center" style={{ background: color + "22" }}>
        <Icon size={18} color={color} />
      </div>
      <div>
        <div className="text-lg font-bold text-[#2D2A4A]">{value}</div>
        <div className="text-xs text-[#2D2A4A99]">{label}</div>
      </div>
    </div>
  );
}

function StudentFormModal({ initial, defaultClass, onClose, onSave }) {
  const [form, setForm] = useState(
    initial || {
      id: "", name: "", class: defaultClass && defaultClass !== "All" ? defaultClass : CLASSES[0], password: "1",
      avatar: "🦁", theme: "coral",
      attendance: { present: 0, total: 0 },
      fees: { total: 0, paid: 0 },
      result: { subjects: [], remarks: "" },
    }
  );
  const [error, setError] = useState("");

  function updateSubject(i, field, val) {
    const subs = [...form.result.subjects];
    subs[i] = { ...subs[i], [field]: field === "name" ? val : Number(val) };
    setForm({ ...form, result: { ...form.result, subjects: subs } });
  }
  function addSubject() {
    setForm({ ...form, result: { ...form.result, subjects: [...form.result.subjects, { name: "", marks: 0, max: 50 }] } });
  }
  function removeSubject(i) {
    setForm({ ...form, result: { ...form.result, subjects: form.result.subjects.filter((_, idx) => idx !== i) } });
  }

  function submit() {
    if (!form.id.trim() || !form.name.trim()) { setError("Roll number aur naam zaroori hai."); return; }
    onSave(form);
  }

  return (
    <div className="fixed inset-0 bg-black/40 flex items-center justify-center p-4 z-50" onClick={onClose}>
      <div className="bg-white rounded-3xl max-w-lg w-full max-h-[88vh] overflow-y-auto p-6" onClick={(e) => e.stopPropagation()}>
        <div className="flex items-center justify-between mb-4">
          <h3 className="text-lg font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>{initial ? "Student Edit Karein" : "Naya Student Jodein"}</h3>
          <button onClick={onClose}><X size={20} className="text-[#2D2A4A99]" /></button>
        </div>

        <div className="grid grid-cols-2 gap-3 mb-3">
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Roll Number</label>
            <input disabled={!!initial} value={form.id} onChange={(e) => setForm({ ...form, id: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1 disabled:bg-[#f5f3ee]" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Naam</label>
            <input value={form.name} onChange={(e) => setForm({ ...form, name: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Class</label>
            <select value={form.class} onChange={(e) => setForm({ ...form, class: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1 bg-white">
              {CLASSES.map((c) => <option key={c}>{c}</option>)}
            </select>
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Password (1-5)</label>
            <select value={form.password} onChange={(e) => setForm({ ...form, password: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1 bg-white">
              {[1, 2, 3, 4, 5].map((n) => <option key={n} value={n}>{n}</option>)}
            </select>
          </div>
        </div>

        <div className="mb-3">
          <label className="text-xs font-semibold text-[#2D2A4A99]">Avatar</label>
          <div className="flex gap-2 mt-1 flex-wrap">
            {AVATARS.map((a) => (
              <button key={a} onClick={() => setForm({ ...form, avatar: a })} className="w-9 h-9 rounded-full text-lg flex items-center justify-center" style={{ background: form.avatar === a ? "#FFD166" : "#F5F3EE" }}>{a}</button>
            ))}
          </div>
        </div>
        <div className="mb-3">
          <label className="text-xs font-semibold text-[#2D2A4A99]">Theme</label>
          <div className="flex gap-2 mt-1 flex-wrap">
            {Object.entries(THEMES).map(([key, t]) => (
              <button key={key} onClick={() => setForm({ ...form, theme: key })} className="px-3 py-1.5 rounded-full text-xs font-semibold flex items-center gap-1" style={{ background: t.bg, color: t.accent, border: form.theme === key ? `2px solid ${t.accent}` : "2px solid transparent" }}>
                {form.theme === key && <Check size={12} />} {t.name}
              </button>
            ))}
          </div>
        </div>

        <div className="grid grid-cols-2 gap-3 mb-3">
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Attendance - Present</label>
            <input type="number" min="0" value={form.attendance.present} onChange={(e) => setForm({ ...form, attendance: { ...form.attendance, present: Number(e.target.value) } })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Attendance - Total Din</label>
            <input type="number" min="0" value={form.attendance.total} onChange={(e) => setForm({ ...form, attendance: { ...form.attendance, total: Number(e.target.value) } })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Total Fees (₹)</label>
            <input type="number" min="0" value={form.fees.total} onChange={(e) => setForm({ ...form, fees: { ...form.fees, total: Number(e.target.value) } })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Fees Paid (₹)</label>
            <input type="number" min="0" value={form.fees.paid} onChange={(e) => setForm({ ...form, fees: { ...form.fees, paid: Number(e.target.value) } })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
        </div>

        <div className="mb-3">
          <div className="flex items-center justify-between">
            <label className="text-xs font-semibold text-[#2D2A4A99]">Result - Subjects</label>
            <button onClick={addSubject} className="text-xs font-semibold text-[#FF6B6B] flex items-center gap-1"><Plus size={13} /> Subject Jodein</button>
          </div>
          <div className="space-y-2 mt-2">
            {form.result.subjects.map((s, i) => (
              <div key={i} className="flex gap-2 items-center">
                <input placeholder="Subject" value={s.name} onChange={(e) => updateSubject(i, "name", e.target.value)} className="flex-1 px-2 py-1.5 rounded-lg border text-sm" />
                <input type="number" placeholder="Marks" value={s.marks} onChange={(e) => updateSubject(i, "marks", e.target.value)} className="w-16 px-2 py-1.5 rounded-lg border text-sm" />
                <span className="text-xs text-[#2D2A4A99]">/</span>
                <input type="number" placeholder="Max" value={s.max} onChange={(e) => updateSubject(i, "max", e.target.value)} className="w-16 px-2 py-1.5 rounded-lg border text-sm" />
                <button onClick={() => removeSubject(i)}><Trash2 size={15} className="text-[#FF6B6B]" /></button>
              </div>
            ))}
          </div>
        </div>

        <div className="mb-4">
          <label className="text-xs font-semibold text-[#2D2A4A99]">Teacher Remarks</label>
          <textarea value={form.result.remarks} onChange={(e) => setForm({ ...form, result: { ...form.result, remarks: e.target.value } })} className="w-full px-3 py-2 rounded-lg border mt-1 text-sm" rows={2} />
        </div>

        {error && <div className="text-sm text-[#FF6B6B] font-medium mb-3">{error}</div>}
        <button onClick={submit} className="w-full py-3 rounded-xl font-bold text-white flex items-center justify-center gap-2" style={{ background: "#2D2A4A" }}>
          <Save size={16} /> Save Karein
        </button>
      </div>
    </div>
  );
}

function TeacherFormModal({ initial, onClose, onSave }) {
  const [form, setForm] = useState(initial || { id: "", name: "", password: "", assignedClass: "All" });
  const [error, setError] = useState("");
  function submit() {
    if (!form.id.trim() || !form.name.trim() || !form.password.trim()) { setError("Sabhi fields bharein."); return; }
    onSave(form);
  }
  return (
    <div className="fixed inset-0 bg-black/40 flex items-center justify-center p-4 z-50" onClick={onClose}>
      <div className="bg-white rounded-3xl max-w-sm w-full p-6" onClick={(e) => e.stopPropagation()}>
        <div className="flex items-center justify-between mb-4">
          <h3 className="text-lg font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>{initial ? "Teacher Edit Karein" : "Naya Teacher Jodein"}</h3>
          <button onClick={onClose}><X size={20} className="text-[#2D2A4A99]" /></button>
        </div>
        <div className="space-y-3">
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Teacher ID</label>
            <input disabled={!!initial} value={form.id} onChange={(e) => setForm({ ...form, id: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1 disabled:bg-[#f5f3ee]" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Naam</label>
            <input value={form.name} onChange={(e) => setForm({ ...form, name: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Password</label>
            <input value={form.password} onChange={(e) => setForm({ ...form, password: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1" />
          </div>
          <div>
            <label className="text-xs font-semibold text-[#2D2A4A99]">Assigned Class</label>
            <select value={form.assignedClass} onChange={(e) => setForm({ ...form, assignedClass: e.target.value })} className="w-full px-3 py-2 rounded-lg border mt-1 bg-white">
              <option value="All">Sabhi Classes</option>
              {CLASSES.map((c) => <option key={c}>{c}</option>)}
            </select>
          </div>
        </div>
        {error && <div className="text-sm text-[#FF6B6B] font-medium mt-3">{error}</div>}
        <button onClick={submit} className="w-full py-3 rounded-xl font-bold text-white flex items-center justify-center gap-2 mt-4" style={{ background: "#2D2A4A" }}>
          <Save size={16} /> Save Karein
        </button>
      </div>
    </div>
  );
}

function ConfirmDelete({ label, onCancel, onConfirm }) {
  return (
    <div className="fixed inset-0 bg-black/40 flex items-center justify-center p-4 z-50" onClick={onCancel}>
      <div className="bg-white rounded-2xl max-w-xs w-full p-5" onClick={(e) => e.stopPropagation()}>
        <p className="text-sm text-[#2D2A4A] mb-4">Kya aap sach me <b>{label}</b> ko delete karna chahte hain?</p>
        <div className="flex gap-2">
          <button onClick={onCancel} className="flex-1 py-2 rounded-xl border font-semibold text-sm">Cancel</button>
          <button onClick={onConfirm} className="flex-1 py-2 rounded-xl text-white font-semibold text-sm" style={{ background: "#FF6B6B" }}>Delete</button>
        </div>
      </div>
    </div>
  );
}

function StudentRow({ s, onEdit, onDelete }) {
  const due = s.fees.total - s.fees.paid;
  return (
    <div className="flex items-center gap-3 bg-white rounded-2xl p-3 shadow-sm border border-[#00000008]">
      <div className="w-10 h-10 rounded-full flex items-center justify-center text-lg shrink-0" style={{ background: THEMES[s.theme]?.bg || "#F5F3EE" }}>{s.avatar}</div>
      <div className="flex-1 min-w-0">
        <div className="font-semibold text-[#2D2A4A] text-sm truncate">{s.name} <span className="text-xs font-normal text-[#2D2A4A99]">#{s.id}</span></div>
        <div className="text-xs text-[#2D2A4A99]">{s.class} • Attendance {pct(s.attendance.present, s.attendance.total)}% • Fees due ₹{due}</div>
      </div>
      <button onClick={() => onEdit(s)} className="p-2 rounded-lg bg-[#F5F3EE]"><Pencil size={15} className="text-[#2D2A4A]" /></button>
      <button onClick={() => onDelete(s)} className="p-2 rounded-lg bg-[#FFEAEA]"><Trash2 size={15} className="text-[#FF6B6B]" /></button>
    </div>
  );
}

function AdminOrTeacherDashboard({ session, data, persist }) {
  const [tab, setTab] = useState("students");
  const [editing, setEditing] = useState(null);
  const [adding, setAdding] = useState(false);
  const [deleting, setDeleting] = useState(null);
  const [teacherEditing, setTeacherEditing] = useState(null);
  const [teacherAdding, setTeacherAdding] = useState(false);
  const [teacherDeleting, setTeacherDeleting] = useState(null);

  const isTeacher = session.role === "teacher";
  const myClass = session.assignedClass;
  const visibleStudents = isTeacher && myClass !== "All"
    ? data.students.filter((s) => s.class === myClass)
    : data.students;

  const totalFeesDue = data.students.reduce((sum, s) => sum + (s.fees.total - s.fees.paid), 0);
  const totalFeesCollected = data.students.reduce((sum, s) => sum + s.fees.paid, 0);
  const avgAttendance = data.students.length
    ? Math.round(data.students.reduce((sum, s) => sum + pct(s.attendance.present, s.attendance.total), 0) / data.students.length)
    : 0;

  function saveStudent(form) {
    const exists = data.students.some((s) => s.id === form.id);
    let next;
    if (editing) {
      next = { ...data, students: data.students.map((s) => (s.id === editing.id ? form : s)) };
    } else {
      if (exists) { alert("Yeh roll number pehle se maujood hai."); return; }
      next = { ...data, students: [...data.students, form] };
    }
    persist(next);
    setEditing(null);
    setAdding(false);
  }

  function deleteStudent(s) {
    persist({ ...data, students: data.students.filter((x) => x.id !== s.id) });
    setDeleting(null);
  }

  function saveTeacher(form) {
    let next;
    if (teacherEditing) {
      next = { ...data, teachers: data.teachers.map((t) => (t.id === teacherEditing.id ? form : t)) };
    } else {
      if (data.teachers.some((t) => t.id === form.id)) { alert("Yeh Teacher ID pehle se maujood hai."); return; }
      next = { ...data, teachers: [...data.teachers, form] };
    }
    persist(next);
    setTeacherEditing(null);
    setTeacherAdding(false);
  }

  function deleteTeacher(t) {
    persist({ ...data, teachers: data.teachers.filter((x) => x.id !== t.id) });
    setTeacherDeleting(null);
  }

  return (
    <div className="max-w-3xl mx-auto p-4 md:p-6">
      <div className="grid grid-cols-3 gap-3 mb-5">
        <StatCard label="Total Students" value={data.students.length} icon={GraduationCap} color="#FF6B6B" />
        <StatCard label="Avg Attendance" value={avgAttendance + "%"} icon={ClipboardCheck} color="#2FB6A8" />
        <StatCard label="Fees Due (₹)" value={totalFeesDue} icon={Wallet} color="#F2A93B" />
      </div>

      <div className="flex gap-1 mb-4 bg-[#00000006] p-1 rounded-2xl w-fit">
        <TabButton active={tab === "students"} onClick={() => setTab("students")} icon={GraduationCap} label="Students" />
        {!isTeacher && <TabButton active={tab === "teachers"} onClick={() => setTab("teachers")} icon={Users} label="Teachers" />}
      </div>

      {tab === "students" && (
        <div>
          <div className="flex items-center justify-between mb-3">
            <h3 className="font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>
              {isTeacher && myClass !== "All" ? `${myClass} - Students` : "Sabhi Students"}
            </h3>
            <button onClick={() => setAdding(true)} className="flex items-center gap-1.5 px-3 py-2 rounded-xl text-white text-sm font-semibold" style={{ background: "#FF6B6B" }}>
              <Plus size={15} /> Add Student
            </button>
          </div>
          <div className="space-y-2">
            {visibleStudents.length === 0 && <p className="text-sm text-[#2D2A4A99] text-center py-8">Abhi koi student nahi hai. Add karein.</p>}
            {visibleStudents.map((s) => (
              <StudentRow key={s.id} s={s} onEdit={setEditing} onDelete={setDeleting} />
            ))}
          </div>
        </div>
      )}

      {tab === "teachers" && !isTeacher && (
        <div>
          <div className="flex items-center justify-between mb-3">
            <h3 className="font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>Sabhi Teachers</h3>
            <button onClick={() => setTeacherAdding(true)} className="flex items-center gap-1.5 px-3 py-2 rounded-xl text-white text-sm font-semibold" style={{ background: "#FF6B6B" }}>
              <Plus size={15} /> Add Teacher
            </button>
          </div>
          <div className="space-y-2">
            {data.teachers.length === 0 && <p className="text-sm text-[#2D2A4A99] text-center py-8">Abhi koi teacher nahi hai. Add karein.</p>}
            {data.teachers.map((t) => (
              <div key={t.id} className="flex items-center gap-3 bg-white rounded-2xl p-3 shadow-sm border border-[#00000008]">
                <div className="w-10 h-10 rounded-full flex items-center justify-center text-lg shrink-0" style={{ background: "#DEF7F4" }}>👩‍🏫</div>
                <div className="flex-1 min-w-0">
                  <div className="font-semibold text-[#2D2A4A] text-sm truncate">{t.name} <span className="text-xs font-normal text-[#2D2A4A99]">#{t.id}</span></div>
                  <div className="text-xs text-[#2D2A4A99]">Class: {t.assignedClass}</div>
                </div>
                <button onClick={() => setTeacherEditing(t)} className="p-2 rounded-lg bg-[#F5F3EE]"><Pencil size={15} className="text-[#2D2A4A]" /></button>
                <button onClick={() => setTeacherDeleting(t)} className="p-2 rounded-lg bg-[#FFEAEA]"><Trash2 size={15} className="text-[#FF6B6B]" /></button>
              </div>
            ))}
          </div>
        </div>
      )}

      {(editing || adding) && (
        <StudentFormModal
          initial={editing}
          defaultClass={isTeacher ? myClass : undefined}
          onClose={() => { setEditing(null); setAdding(false); }}
          onSave={saveStudent}
        />
      )}
      {deleting && <ConfirmDelete label={deleting.name} onCancel={() => setDeleting(null)} onConfirm={() => deleteStudent(deleting)} />}

      {(teacherEditing || teacherAdding) && (
        <TeacherFormModal
          initial={teacherEditing}
          onClose={() => { setTeacherEditing(null); setTeacherAdding(false); }}
          onSave={saveTeacher}
        />
      )}
      {teacherDeleting && <ConfirmDelete label={teacherDeleting.name} onCancel={() => setTeacherDeleting(null)} onConfirm={() => deleteTeacher(teacherDeleting)} />}
    </div>
  );
}

function StudentDashboard({ session, data, persist }) {
  const student = data.students.find((s) => s.id === session.id);
  if (!student) return <div className="p-8 text-center text-[#2D2A4A99]">Data nahi mila.</div>;

  const theme = THEMES[student.theme] || THEMES.coral;
  const attPct = pct(student.attendance.present, student.attendance.total);
  const due = student.fees.total - student.fees.paid;
  const totalMarks = student.result.subjects.reduce((s, x) => s + x.marks, 0);
  const totalMax = student.result.subjects.reduce((s, x) => s + x.max, 0);
  const resultPct = pct(totalMarks, totalMax);

  function updateSelf(patch) {
    const next = { ...data, students: data.students.map((s) => (s.id === student.id ? { ...s, ...patch } : s)) };
    persist(next);
  }

  return (
    <div className="max-w-2xl mx-auto p-4 md:p-6 space-y-4">
      <div className="rounded-3xl p-5 flex items-center gap-4" style={{ background: theme.bg }}>
        <div className="w-16 h-16 rounded-full bg-white flex items-center justify-center text-3xl shadow-sm">{student.avatar}</div>
        <div>
          <div className="text-lg font-bold text-[#2D2A4A]" style={{ fontFamily: "'Fredoka', sans-serif" }}>{student.name}</div>
          <div className="text-sm text-[#2D2A4A99]">Roll No. {student.id} • {student.class}</div>
        </div>
      </div>

      <div className="bg-white rounded-2xl p-5 shadow-sm border border-[#00000008]">
        <div className="flex items-center gap-2 mb-2 text-[#2D2A4A] font-semibold"><ClipboardCheck size={17} color={theme.accent} /> Attendance</div>
        <ProgressBar value={attPct} color={theme.accent} />
        <div className="text-sm text-[#2D2A4A99] mt-2">{student.attendance.present} / {student.attendance.total} din present — {attPct}%</div>
      </div>

      <div className="bg-white rounded-2xl p-5 shadow-sm border border-[#00000008]">
        <div className="flex items-center gap-2 mb-2 text-[#2D2A4A] font-semibold"><Wallet size={17} color={theme.accent} /> Fees</div>
        <div className="grid grid-cols-3 gap-2 text-center">
          <div><div className="text-lg font-bold text-[#2D2A4A]">₹{student.fees.total}</div><div className="text-xs text-[#2D2A4A99]">Total</div></div>
          <div><div className="text-lg font-bold text-[#3FA65C]">₹{student.fees.paid}</div><div className="text-xs text-[#2D2A4A99]">Paid</div></div>
          <div><div className="text-lg font-bold text-[#FF6B6B]">₹{due}</div><div className="text-xs text-[#2D2A4A99]">Due</div></div>
        </div>
      </div>

      <div className="bg-white rounded-2xl p-5 shadow-sm border border-[#00000008]">
        <div className="flex items-center gap-2 mb-3 text-[#2D2A4A] font-semibold"><Award size={17} color={theme.accent} /> Result</div>
        {student.result.subjects.length === 0 ? (
          <p className="text-sm text-[#2D2A4A99]">Result abhi update nahi hua hai.</p>
        ) : (
          <div className="space-y-2">
            {student.result.subjects.map((sub, i) => (
              <div key={i} className="flex items-center justify-between text-sm">
                <span className="text-[#2D2A4A]">{sub.name}</span>
                <span className="font-semibold text-[#2D2A4A99]">{sub.marks} / {sub.max}</span>
              </div>
            ))}
            <div className="pt-2 border-t border-[#00000010] flex items-center justify-between font-bold text-[#2D2A4A]">
              <span>Total</span><span>{totalMarks} / {totalMax} ({resultPct}%)</span>
            </div>
            {student.result.remarks && <p className="text-xs text-[#2D2A4A99] italic mt-2">"{student.result.remarks}"</p>}
          </div>
        )}
      </div>

      <div className="bg-white rounded-2xl p-5 shadow-sm border border-[#00000008]">
        <div className="flex items-center gap-2 mb-3 text-[#2D2A4A] font-semibold"><Smile size={17} color={theme.accent} /> Mera Avatar aur Theme</div>
        <div className="mb-3">
          <div className="text-xs font-semibold text-[#2D2A4A99] mb-1.5">Avatar chuniye</div>
          <div className="flex gap-2 flex-wrap">
            {AVATARS.map((a) => (
              <button key={a} onClick={() => updateSelf({ avatar: a })} className="w-9 h-9 rounded-full text-lg flex items-center justify-center" style={{ background: student.avatar === a ? "#FFD166" : "#F5F3EE" }}>{a}</button>
            ))}
          </div>
        </div>
        <div>
          <div className="text-xs font-semibold text-[#2D2A4A99] mb-1.5">Theme chuniye</div>
          <div className="flex gap-2 flex-wrap">
            {Object.entries(THEMES).map(([key, t]) => (
              <button key={key} onClick={() => updateSelf({ theme: key })} className="px-3 py-1.5 rounded-full text-xs font-semibold flex items-center gap-1" style={{ background: t.bg, color: t.accent, border: student.theme === key ? `2px solid ${t.accent}` : "2px solid transparent" }}>
                {student.theme === key && <Check size={12} />} {t.name}
              </button>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
}

export default function App() {
  const { data, status, persist } = useSchoolData();
  const [session, setSession] = useState(null);

  return (
    <div className="min-h-screen" style={{ background: "#FFF9F0", fontFamily: "'Nunito', sans-serif" }}>
      <style>{`@import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=Nunito:wght@400;600;700&display=swap');`}</style>
      {status === "loading" || !data ? (
        <div className="min-h-screen flex items-center justify-center text-[#2D2A4A]">
          <Loader2 className="animate-spin mr-2" size={20} /> Load ho raha hai...
        </div>
      ) : !session ? (
        <LoginScreen data={data} onLogin={setSession} />
      ) : (
        <>
          <Header session={session} onLogout={() => setSession(null)} />
          {session.role === "student"
            ? <StudentDashboard session={session} data={data} persist={persist} />
            : <AdminOrTeacherDashboard session={session} data={data} persist={persist} />}
        </>
      )}
    </div>
  );
}
