# heloisealmeidal.github.io


<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: var(--font-sans); }
.header { padding: 1.5rem 1.25rem 1rem; border-bottom: 0.5px solid var(--color-border-tertiary); }
.header h1 { font-size: 20px; font-weight: 500; color: var(--color-text-primary); }
.header p { font-size: 13px; color: var(--color-text-secondary); margin-top: 4px; }
.nav { display: flex; gap: 8px; padding: 1rem 1.25rem 0; flex-wrap: wrap; }
.nav button { font-size: 13px; padding: 6px 14px; border-radius: var(--border-radius-md); border: 0.5px solid var(--color-border-secondary); background: transparent; color: var(--color-text-secondary); cursor: pointer; transition: all 0.15s; }
.nav button:hover { background: var(--color-background-secondary); }
.nav button.active { background: var(--color-background-secondary); color: var(--color-text-primary); border-color: var(--color-border-primary); font-weight: 500; }
.content { padding: 1rem 1.25rem 2rem; }
.month-section { display: none; }
.month-section.visible { display: block; }
.day-group { margin-bottom: 1.5rem; }
.day-header { font-size: 14px; font-weight: 500; color: var(--color-text-primary); margin-bottom: 8px; padding-bottom: 6px; border-bottom: 0.5px solid var(--color-border-tertiary); display: flex; align-items: center; gap: 8px; }
.day-tag { font-size: 11px; padding: 2px 8px; border-radius: 20px; font-weight: 400; }
.fri { background: #EEF2FF; color: #3730A3; }
.sat { background: #FFF7ED; color: #9A3412; }
.slots { display: flex; flex-direction: column; gap: 4px; }
.slot { display: grid; grid-template-columns: 110px 1fr; align-items: center; gap: 10px; padding: 6px 10px; border-radius: var(--border-radius-md); border: 0.5px solid var(--color-border-tertiary); background: var(--color-background-primary); }
.slot.intervalo { background: transparent; border-color: transparent; }
.slot.intervalo .subject { color: var(--color-text-tertiary); font-size: 12px; font-style: italic; }
.slot.empty { background: transparent; border-color: transparent; }
.slot.empty .subject { color: var(--color-text-tertiary); font-size: 12px; }
.time { font-size: 12px; color: var(--color-text-secondary); font-variant-numeric: tabular-nums; white-space: nowrap; }
.subject { font-size: 13px; font-weight: 400; display: flex; align-items: center; gap: 6px; }
.dot { width: 9px; height: 9px; border-radius: 50%; flex-shrink: 0; }
.legend { display: flex; flex-wrap: wrap; gap: 8px; padding: 0 1.25rem 1.5rem; }
.legend-item { display: flex; align-items: center; gap: 5px; font-size: 12px; color: var(--color-text-secondary); }
.legend-dot { width: 9px; height: 9px; border-radius: 50%; flex-shrink: 0; }
.especial { background: #FEF3C7; border-color: #FDE68A !important; }
.especial .subject { color: #92400E; font-weight: 500; }
</style>

<h2 class="sr-only" style="position:absolute;left:-9999px">Calendário de Aulas 2026 — Cursinho Popular Nísia Floresta</h2>

<div class="header">
  <h1>Cursinho Popular Nísia Floresta</h1>
  <p>Calendário de Aulas 2026</p>
</div>

<div class="nav" id="nav"></div>

<div class="legend" id="legend"></div>

<div class="content" id="content"></div>

<script>
const COLORS = {
  "Matemática":        { dot: "#378ADD", text: "#0C447C" },
  "Física":            { dot: "#E24B4A", text: "#A32D2D" },
  "Química":           { dot: "#EF9F27", text: "#854F0B" },
  "Biologia":          { dot: "#639922", text: "#3B6D11" },
  "História":          { dot: "#D85A30", text: "#993C1D" },
  "Geografia":         { dot: "#1D9E75", text: "#0F6E56" },
  "Linguagens":        { dot: "#7F77DD", text: "#3C3489" },
  "Redação":           { dot: "#D4537E", text: "#993556" },
  "Sociologia/Filosofia": { dot: "#888780", text: "#444441" },
  "Filosofia/Sociologia": { dot: "#888780", text: "#444441" },
  "Debate Público":    { dot: "#533AB7", text: "#26215C" },
};

function cleanSubject(s) {
  if (!s) return null;
  s = s.replace(/\(.*?\)/g, "").trim();
  if (!s) return null;
  return s;
}

const MONTHS = {
  "Maio": [
    { date: "08/05", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "História" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "História" },
    ]},
    { date: "09/05", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Matemática" },
      { t: "08:50–09:40", s: "Matemática" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Química" },
      { t: "10:50–11:40", s: "Química" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Redação" },
      { t: "15:50–16:40", s: "Redação" },
    ]},
    { date: "15/05", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Física" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Química" },
    ]},
    { date: "16/05", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Redação" },
      { t: "08:50–09:40", s: "Redação" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Geografia" },
      { t: "10:50–11:40", s: "Geografia" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "História" },
      { t: "13:50–14:40", s: "História" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Matemática" },
      { t: "15:50–16:40", s: "Matemática" },
    ]},
    { date: "22/05", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Biologia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Biologia" },
    ]},
    { date: "23/05", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "História" },
      { t: "08:50–09:40", s: "História" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Geografia" },
      { t: "15:50–16:40", s: "Geografia" },
    ]},
    { date: "29/05", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Química" },
      { t: "19:40–20:20", s: "Sociologia/Filosofia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Sociologia/Filosofia" },
    ]},
    { date: "30/05", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Redação" },
      { t: "08:50–09:40", s: "Redação" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Linguagens" },
      { t: "10:50–11:40", s: "Linguagens" },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
  ],
  "Junho": [
    { date: "05/06", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Biologia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Biologia" },
    ]},
    { date: "06/06", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "História" },
      { t: "08:50–09:40", s: "História" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Geografia" },
      { t: "15:50–16:40", s: "Geografia" },
    ]},
    { date: "12/06", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Química" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Redação" },
    ]},
    { date: "13/06", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Matemática" },
      { t: "08:50–09:40", s: "Matemática" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Linguagens" },
      { t: "10:50–11:40", s: "Linguagens" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "História" },
      { t: "13:50–14:40", s: "História" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Biologia" },
      { t: "15:50–16:40", s: "Biologia" },
    ]},
    { date: "19/06", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "Química" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Química" },
    ]},
    { date: "20/06", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Redação" },
      { t: "08:50–09:40", s: "Redação" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Geografia" },
      { t: "15:50–16:40", s: "Geografia" },
    ]},
    { date: "26/06", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Redação" },
    ]},
    { date: "27/06", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "História" },
      { t: "08:50–09:40", s: "História" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Biologia" },
      { t: "10:50–11:40", s: "Biologia" },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
  ],
  "Julho": [
    { date: "03/07", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "Química" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Química" },
    ]},
    { date: "04/07", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Biologia" },
      { t: "08:50–09:40", s: "Biologia" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Química" },
      { t: "10:50–11:40", s: "Química" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Matemática" },
      { t: "13:50–14:40", s: "Matemática" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Linguagens" },
      { t: "15:50–16:40", s: "Linguagens" },
    ]},
    { date: "10/07", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Redação" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "História" },
    ]},
    { date: "11/07", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Física" },
      { t: "08:50–09:40", s: "Física" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Biologia" },
      { t: "15:50–16:40", s: "Biologia" },
    ]},
    { date: "17/07", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "Física" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Física" },
    ]},
    { date: "18/07", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Geografia" },
      { t: "08:50–09:40", s: "Geografia" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Química" },
      { t: "10:50–11:40", s: "Química" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "História" },
      { t: "13:50–14:40", s: "História" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Geografia" },
      { t: "15:50–16:40", s: "Geografia" },
    ]},
    { date: "24/07", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Matemática" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Matemática" },
    ]},
    { date: "25/07", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Física" },
      { t: "08:50–09:40", s: "Física" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Biologia" },
      { t: "10:50–11:40", s: "Biologia" },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
    { date: "31/07", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "História" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Biologia" },
    ]},
  ],
  "Agosto": [
    { date: "01/08", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Linguagens" },
      { t: "08:50–09:40", s: "Linguagens" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Linguagens" },
      { t: "10:50–11:40", s: "Linguagens" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Geografia" },
      { t: "13:50–14:40", s: "Geografia" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Sociologia/Filosofia" },
      { t: "15:50–16:40", s: "Sociologia/Filosofia" },
    ]},
    { date: "07/08", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Redação" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Química" },
    ]},
    { date: "08/08", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Biologia" },
      { t: "08:50–09:40", s: "Biologia" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Química" },
      { t: "13:50–14:40", s: "Química" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Biologia" },
      { t: "15:50–16:40", s: "Biologia" },
    ]},
    { date: "14/08", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Geografia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Geografia" },
    ]},
    { date: "15/08", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Linguagens" },
      { t: "08:50–09:40", s: "Linguagens" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Linguagens" },
      { t: "10:50–11:40", s: "Linguagens" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "História" },
      { t: "13:50–14:40", s: "História" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Matemática" },
      { t: "15:50–16:40", s: "Linguagens" },
    ]},
    { date: "21/08", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Geografia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Geografia" },
    ]},
    { date: "22/08", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Biologia" },
      { t: "08:50–09:40", s: "Biologia" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Matemática" },
      { t: "13:50–14:40", s: "Matemática" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Redação" },
      { t: "15:50–16:40", s: "Redação" },
    ]},
    { date: "28/08", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "Sociologia/Filosofia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Física" },
    ]},
    { date: "29/08", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Química" },
      { t: "08:50–09:40", s: "Química" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Matemática" },
      { t: "10:50–11:40", s: "Matemática" },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
  ],
  "Setembro": [
    { date: "04/09", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Redação" },
    ]},
    { date: "05/09", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Matemática" },
      { t: "08:50–09:40", s: "Matemática" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "Física" },
      { t: "10:50–11:40", s: "Física" },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Redação" },
      { t: "13:50–14:40", s: "Redação" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Matemática" },
      { t: "15:50–16:40", s: "Matemática" },
    ]},
    { date: "11/09", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Química" },
      { t: "19:40–20:20", s: "Química" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Sociologia/Filosofia" },
    ]},
    { date: "12/09", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Química" },
      { t: "08:50–09:40", s: "Química" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Geografia" },
      { t: "13:50–14:40", s: "Geografia" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Linguagens" },
      { t: "15:50–16:40", s: "Linguagens" },
    ]},
    { date: "18/09", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Química" },
      { t: "19:40–20:20", s: "Química" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Redação" },
    ]},
    { date: "19/09", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "Física" },
      { t: "08:50–09:40", s: "Física" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Biologia" },
      { t: "15:50–16:40", s: "Biologia" },
    ]},
    { date: "25/09", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Sociologia/Filosofia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Sociologia/Filosofia" },
    ]},
    { date: "26/09", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: "História" },
      { t: "08:50–09:40", s: "História" },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: "História" },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
  ],
  "Outubro": [
    { date: "02/10", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Sociologia/Filosofia" },
      { t: "19:40–20:20", s: "Sociologia/Filosofia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "História" },
    ]},
    { date: "09/10", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Física" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Sociologia/Filosofia" },
    ]},
    { date: "10/10", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: null, empty: true },
      { t: "08:50–09:40", s: null, empty: true },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Linguagens" },
      { t: "13:50–14:40", s: "Linguagens" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "História" },
      { t: "15:50–16:40", s: "História" },
    ]},
    { date: "16/10", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Física" },
      { t: "19:40–20:20", s: "Geografia" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Geografia" },
    ]},
    { date: "17/10", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: null, empty: true },
      { t: "08:50–09:40", s: null, empty: true },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
    ]},
    { date: "23/10", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Redação" },
      { t: "19:40–20:20", s: "Redação" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Biologia" },
    ]},
    { date: "24/10", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: null, empty: true },
      { t: "08:50–09:40", s: null, empty: true },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–13:00", s: null, intervalo: true },
      { t: "13:00–13:50", s: "Matemática" },
      { t: "13:50–14:40", s: "Matemática" },
      { t: "14:40–15:00", s: null, intervalo: true },
      { t: "15:00–15:50", s: "Linguagens" },
      { t: "15:50–16:40", s: "Linguagens" },
    ]},
    { date: "30/10", weekday: "Sexta-feira", slots: [
      { t: "19:00–19:40", s: "Química" },
      { t: "19:40–20:20", s: "Química" },
      { t: "20:20–20:40", s: null, intervalo: true },
      { t: "20:40–21:30", s: "Redação" },
    ]},
    { date: "31/10", weekday: "Sábado", slots: [
      { t: "08:00–08:50", s: null, empty: true },
      { t: "08:50–09:40", s: null, empty: true },
      { t: "09:40–10:00", s: null, intervalo: true },
      { t: "10:00–10:50", s: null, empty: true },
      { t: "10:50–11:40", s: null, empty: true },
      { t: "11:40–14:00", s: null, intervalo: true },
      { t: "14:00–16:00", s: "Debate Público", especial: true },
    ]},
  ],
  "Novembro": [
    { date: "07/11", weekday: "Sábado", slots: [
      { t: "Dia todo", s: "Aulão de Ciências Humanas, Linguagens e Redação", especial: true },
    ]},
    { date: "14/11", weekday: "Sábado", slots: [
      { t: "Dia todo", s: "Aulão de Matemática e Ciências da Natureza", especial: true },
    ]},
  ],
};

const monthOrder = ["Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro"];

function renderSubject(slot) {
  if (slot.intervalo) {
    return `<div class="slot intervalo"><span class="time">${slot.t}</span><span class="subject">intervalo</span></div>`;
  }
  if (slot.empty || !slot.s) {
    return `<div class="slot empty"><span class="time">${slot.t}</span><span class="subject" style="color:var(--color-text-tertiary);font-size:12px;">a definir</span></div>`;
  }
  const subj = slot.s;
  const color = COLORS[subj] || { dot: "#888", text: "#444" };
  const extra = slot.especial ? 'style="background:#FEF9EC;border-color:#FDE68A;"' : '';
  const textStyle = slot.especial ? `color:${color.text};font-weight:500;` : `color:var(--color-text-primary);`;
  return `<div class="slot" ${extra}>
    <span class="time">${slot.t}</span>
    <span class="subject">
      <span class="dot" style="background:${color.dot}"></span>
      <span style="${textStyle}">${subj}</span>
    </span>
  </div>`;
}

function renderMonth(month) {
  const days = MONTHS[month];
  return days.map(day => {
    const isFri = day.weekday.toLowerCase().includes("sexta");
    const tagClass = isFri ? "fri" : "sat";
    const tagLabel = isFri ? "Sexta-feira" : "Sábado";
    return `<div class="day-group">
      <div class="day-header">
        <strong>${day.date}</strong>
        <span class="day-tag ${tagClass}">${tagLabel}</span>
      </div>
      <div class="slots">${day.slots.map(renderSubject).join("")}</div>
    </div>`;
  }).join("");
}

const nav = document.getElementById("nav");
const content = document.getElementById("content");

monthOrder.forEach((m, i) => {
  const btn = document.createElement("button");
  btn.textContent = m;
  btn.dataset.month = m;
  if (i === 0) btn.classList.add("active");
  btn.onclick = () => {
    document.querySelectorAll(".nav button").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    document.querySelectorAll(".month-section").forEach(s => s.classList.remove("visible"));
    document.getElementById("sec-" + m).classList.add("visible");
  };
  nav.appendChild(btn);

  const sec = document.createElement("div");
  sec.className = "month-section" + (i === 0 ? " visible" : "");
  sec.id = "sec-" + m;
  sec.innerHTML = renderMonth(m);
  content.appendChild(sec);
});

const legend = document.getElementById("legend");
const shown = new Set();
Object.entries(COLORS).forEach(([name, c]) => {
  const key = name === "Filosofia/Sociologia" ? "Sociologia/Filosofia" : name;
  if (shown.has(key)) return;
  shown.add(key);
  const item = document.createElement("div");
  item.className = "legend-item";
  item.innerHTML = `<span class="legend-dot" style="background:${c.dot}"></span><span>${key}</span>`;
  legend.appendChild(item);
});
</script>
