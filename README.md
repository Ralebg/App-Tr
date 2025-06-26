# App-Tr
Aplicación de Trading
import React, { useState } from "react";
import {
  LineChart, Line, BarChart, Bar, XAxis, YAxis,
  Tooltip, ResponsiveContainer, CartesianGrid, Legend
} from "recharts";

const fullData = {
  "EE.UU.": {
    "2025-05-29": {
      line: [
        { name: "ASML", valor: 945 },
        { name: "Solana", valor: 148 },
        { name: "Tesla", valor: 177 }
      ],
      bar: [
        { name: "Tesla", inversion: 1700, subida: 6 },
        { name: "ASML", inversion: 1600, subida: 4 },
        { name: "Solana", inversion: 1200, subida: 6 }
      ]
    }
  },
  Asia: {
    "2025-05-29": {
      line: [
        { name: "Alibaba", valor: 82.3 },
        { name: "TSMC", valor: 118 },
        { name: "Toyota", valor: 156 }
      ],
      bar: [
        { name: "Alibaba", inversion: 1200, subida: 5 },
        { name: "TSMC", inversion: 1000, subida: 3 },
        { name: "Toyota", inversion: 900, subida: 2.5 }
      ]
    }
  },
  Europa: {
    "2025-05-29": {
      line: [
        { name: "LVMH", valor: 828 },
        { name: "Iberdrola", valor: 11.8 },
        { name: "Siemens", valor: 179 }
      ],
      bar: [
        { name: "LVMH", inversion: 1300, subida: 3 },
        { name: "Siemens", inversion: 1100, subida: 2 },
        { name: "Iberdrola", inversion: 700, subida: 1.5 }
      ]
    }
  }
};

function WelcomeScreen({ onEnter }) {
  return (
    <div className="p-6 text-center relative flex flex-col justify-center items-center min-h-screen">
      <h1 className="text-3xl font-bold mb-4">Bienvenido a App Rendimiento</h1>
      <p className="text-gray-600 mb-2 text-center">Gesti贸n personalizada y segura de tu perfil inversor</p>
      <p className="text-xs text-gray-400 mb-6 text-center">Versi贸n 1.1</p>
      <button
        onClick={onEnter}
        className="absolute top-4 left-4 bg-gray-100 p-2 rounded-full shadow hover:bg-gray-200"
        title="Entrar al Panel Principal"
      >
        馃彔
      </button>
    </div>
  );
}

export default function PanelPrincipal() {
  const [view, setView] = useState("welcome");
  const [region, setRegion] = useState("EE.UU.");
  const [fecha, setFecha] = useState("2025-05-29");
  const [showFilters, setShowFilters] = useState(false);
  const [comportamientoAsc, setComportamientoAsc] = useState(false);
  const [comportamientoNeg, setComportamientoNeg] = useState(false);
  const [costoAccion, setCostoAccion] = useState(false);

  const data = fullData[region]?.[fecha] || { line: [], bar: [] };
  const sortedLine = [...data.line].sort((a, b) => {
    if (costoAccion) return b.valor - a.valor;
    if (comportamientoAsc) return b.valor - a.valor;
    if (comportamientoNeg) return a.valor - b.valor;
    return 0;
  });
  const sortedBar = [...data.bar];

  if (view === "welcome") return <WelcomeScreen onEnter={() => setView("panel")} />;

  return (
    <div className="p-6 relative">
      <div className="relative mb-2">
        <button onClick={() => setView("welcome")} className="absolute top-1 left-1 bg-gray-100 p-1 rounded-full shadow hover:bg-gray-200" title="Volver">猬咃笍</button>
        <h1 className="text-3xl font-bold text-center w-full mt-36 mb-6">Panel Principal</h1>
        <button
          onClick={() => setShowFilters(!showFilters)}
          className="absolute top-[10.5rem] left-20 bg-white p-2 rounded shadow-md border border-gray-300 hover:bg-gray-50 flex items-center gap-2 text-sm z-50"
          title="Mostrar/Ocultar Filtros"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth={1.5} stroke="currentColor" className="w-4 h-4">
            <path strokeLinecap="round" strokeLinejoin="round" d="M3 4.5h18m-18 7.5h18m-18 7.5h18" />
          </svg>
        </button>
      </div>

      {showFilters && (
        <div className="absolute top-[13.3rem] left-20 bg-white border border-gray-300 rounded-lg shadow-xl p-4 w-80 z-40 transition-all duration-200 ease-out">
          <div className="space-y-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Bolsa</label>
              <select value={region} onChange={e => setRegion(e.target.value)} className="w-full border border-gray-300 rounded px-2 py-1 text-sm">
                <option>EE.UU.</option>
                <option>Asia</option>
                <option>Europa</option>
              </select>
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Fecha</label>
              <input type="date" value={fecha} onChange={e => setFecha(e.target.value)} className="w-full border border-gray-300 rounded px-2 py-1 text-sm" />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Hora</label>
              <input type="time" className="w-full border border-gray-300 rounded px-2 py-1 text-sm" />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">Activo</label>
              <input type="text" placeholder="Ej. Tesla, Solana..." className="w-full border border-gray-300 rounded px-2 py-1 text-sm" />
            </div>
            <div className="space-y-2">
              <label className="flex items-center text-sm gap-2">
                <input type="checkbox" checked={costoAccion} onChange={() => setCostoAccion(!costoAccion)} />
                Costo de la acci贸n
              </label>
              <label className="flex items-center text-sm gap-2">
                <input type="checkbox" checked={comportamientoAsc} onChange={() => setComportamientoAsc(!comportamientoAsc)} />
                Comportamiento ascendente
              </label>
              <label className="flex items-center text-sm gap-2">
                <input type="checkbox" checked={comportamientoNeg} onChange={() => setComportamientoNeg(!comportamientoNeg)} />
                Comportamiento negativo
              </label>
            </div>
          </div>
        </div>
      )}

      <div className="mt-36 mb-10">
        <h2 className="font-semibold text-lg mb-2">Gr谩fico de comportamiento</h2>
        <ResponsiveContainer width="100%" height={300}>
          <LineChart data={sortedLine}>
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="name" />
            <YAxis />
            <Tooltip />
            <Legend />
            <Line type="monotone" dataKey="valor" stroke="#8884d8" />
          </LineChart>
        </ResponsiveContainer>
      </div>

      <div className="mt-20 mb-10">
        <h2 className="font-semibold text-lg mb-2">Acciones recomendadas e inversi贸n</h2>
        <ResponsiveContainer width="100%" height={300}>
          <BarChart data={sortedBar}>
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="name" />
            <YAxis />
            <Tooltip />
            <Legend />
            <Bar dataKey="inversion" fill="#82ca9d" />
            <Bar dataKey="subida" fill="#8884d8" />
          </BarChart>
        </ResponsiveContainer>
      </div>

      <p className="text-sm text-gray-600 mt-4">
        Filtrado actual: Bolsa: {region} 路 Fecha: {fecha}
      </p>
    </div>
  );
}
