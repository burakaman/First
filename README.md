import React, { useState, useEffect } from "react";

export default function MasalaStockApp() { const [isAuthenticated, setIsAuthenticated] = useState(false); const [securityCode, setSecurityCode] = useState(""); const [masalas, setMasalas] = useState(() => { const saved = localStorage.getItem("masalas"); return saved ? JSON.parse(saved) : []; });

const [form, setForm] = useState({ name: "", quantity: "", unit: "kg", price: "", date: "" });

useEffect(() => { localStorage.setItem("masalas", JSON.stringify(masalas)); }, [masalas]);

const handleChange = (e) => { setForm({ ...form, [e.target.name]: e.target.value }); };

const addMasala = () => { if (!form.name || !form.quantity) return alert("Fill in all details"); setMasalas([...masalas, { ...form, id: Date.now() }]); setForm({ name: "", quantity: "", unit: "kg", price: "", date: "" }); };

const removeMasala = (id) => { setMasalas(masalas.filter((m) => m.id !== id)); };

const handleLogin = () => { if (securityCode === "1234") { setIsAuthenticated(true); } else { alert("Incorrect security code"); } };

if (!isAuthenticated) { return ( <div className="flex items-center justify-center h-screen bg-gray-100"> <div className="bg-white p-6 rounded shadow-md"> <h2 className="text-xl font-bold mb-4">Enter Security Code</h2> <input type="password" value={securityCode} onChange={(e) => setSecurityCode(e.target.value)} placeholder="Security Code" className="border p-2 mb-2 w-full" /> <button
onClick={handleLogin}
className="bg-blue-500 text-white p-2 rounded w-full"
> Login </button> </div> </div> ); }

return ( <div className="p-4 max-w-3xl mx-auto"> <h1 className="text-2xl font-bold mb-4">Masala Stock Manager</h1>

<div className="grid grid-cols-2 gap-2 mb-4">
    <input name="name" value={form.name} onChange={handleChange} placeholder="Masala Name" className="border p-2" />
    <input name="quantity" value={form.quantity} onChange={handleChange} placeholder="Quantity" className="border p-2" />
    <select name="unit" value={form.unit} onChange={handleChange} className="border p-2">
      <option value="kg">kg</option>
      <option value="g">g</option>
      <option value="packet">packet</option>
    </select>
    <input name="price" value={form.price} onChange={handleChange} placeholder="Price" className="border p-2" />
    <input name="date" value={form.date} onChange={handleChange} type="date" className="border p-2" />
    <button onClick={addMasala} className="bg-blue-500 text-white p-2 rounded">Add Masala</button>
  </div>

  <table className="w-full table-auto border">
    <thead>
      <tr className="bg-gray-200">
        <th>Name</th>
        <th>Qty</th>
        <th>Unit</th>
        <th>Price</th>
        <th>Date</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      {masalas.map((m) => (
        <tr key={m.id} className="border-t">
          <td>{m.name}</td>
          <td>{m.quantity}</td>
          <td>{m.unit}</td>
          <td>{m.price}</td>
          <td>{m.date}</td>
          <td><button onClick={() => removeMasala(m.id)} className="text-red-500">Delete</button></td>
        </tr>
      ))}
    </tbody>
  </table>
</div>

); }


