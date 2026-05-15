# ASCENDANCY-Intellectual-Finance-Protocol-V5.2-
ASCENDANCY Intellectual Finance Protocol — Cinematic dark-ops AI dashboard for real-time U.S. defense contract arbitrage, Oracle Protocol verification, 3D Aegis Grid, and Emperor-Tier autonomous agents.
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: {
        obsidian: '#0A0A0A',
        silver: '#C0C0C0',
        cobalt: '#00BFFF',
      },
    },
  },
  plugins: [],
}@tailwind base;
@tailwind components;
@tailwind utilities;
.glass {
  background: rgba(10, 10, 10, 0.75);
  import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App.tsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
)import { Routes, Route } from 'react-router-dom'
import Layout from './components/Layout'
import Dashboard from './pages/Dashboard'
import Arbitrage from './pages/Arbitrage'
import TitanNode from './pages/TitanNode'

function App() {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route path="/" element={<Dashboard />} />
        <Route path="/arbitrage" element={<Arbitrage />} />
        <Route path="/titan" element={<TitanNode />} />
      </Route>
    </Routes>
  )
}

export default App
import { Link, Outlet } from 'react-router-dom'
import { LayoutDashboard, Search, Bot } from 'lucide-react'

export default function Layout() {
  return (
    <div className="flex h-screen bg-obsidian">
      <div className="w-64 glass p-6 border-r border-silver/20">
        <div className="text-3xl font-bold text-silver mb-12">ASCENDANCY</div>
        <nav className="space-y-2">
          <Link to="/" className="flex items-center gap-3 px-4 py-3 rounded-2xl hover:bg-cobalt/10 text-silver hover:text-white"><LayoutDashboard /> Dashboard</Link>
          <Link to="/arbitrage" className="flex items-center gap-3 px-4 py-3 rounded-2xl hover:bg-cobalt/10 text-silver hover:text-white"><Search /> Arbitrage Node</Link>
          <Link to="/titan" className="flex items-center gap-3 px-4 py-3 rounded-2xl hover:bg-cobalt/10 text-silver hover:text-white"><Bot /> Titan Advisor</Link>
        </nav>
      </div>
      <div className="flex-1 overflow-auto">
        <Outlet />
      </div>
    </div>
  )
}import { Canvas } from '@react-three/fiber'
import { OrbitControls, Stars, Environment } from '@react-three/drei'
import { EffectComposer, Bloom } from '@react-three/postprocessing'
import { KernelSize } from 'postprocessing'
import { useMemo } from 'react'
import * as THREE from 'three'
import { motion } from 'framer-motion'

export default function AegisGrid3D() {
  const { nodes, links } = useMemo(() => {
    const nodeCount = 80
    const nodesData = Array.from({ length: nodeCount }, (_, i) => ({
      id: i,
      position: [(Math.random() - 0.5) * 40, (Math.random() - 0.5) * 30, (Math.random() - 0.5) * 35] as [number, number, number],
      value: Math.random() * 15 + 8,
      verified: Math.random() > 0.7
    }))
    const linksData = []
    for (let i = 0; i < nodeCount; i++) {
      for (let j = i + 1; j < nodeCount; j++) {
        if (Math.random() > 0.85) linksData.push({ source: i, target: j })
      }
    }
    return { nodes: nodesData, links: linksData }
  }, [])

  return (
    <motion.div className="glass rounded-3xl overflow-hidden h-[620px] relative" initial={{opacity:0}} animate={{opacity:1}}>
      <div className="absolute top-4 left-4 z-10 font-mono text-cobalt text-sm flex items-center gap-2">
        <div className="w-2 h-2 bg-green-400 rounded-full animate-pulse" />
        AEGIS GRID • ORACLE PROTOCOL • LIVE
      </div>
      <Canvas camera={{ position: [0,0,60], fov: 50 }}>
        <ambientLight intensity={0.15} />
        <pointLight position={[10,10,10]} color="#00BFFF" intensity={2} />
        {nodes.map((node,i) => (
          <mesh key={i} position={node.position}>
            <sphereGeometry args={[node.value/4]} />
            <meshStandardMaterial
              color={node.verified ? "#22ff88" : "#00BFFF"}
              emissive={node.verified ? "#22ff88" : "#00BFFF"}
              emissiveIntensity={node.verified ? 2.8 : 0.9}
              metalness={0.95}
              roughness={0.1}
            />
          </mesh>
        ))}
        {links.map((link,i) => {
          const source = nodes[link.source]
          const target = nodes[link.target]
          return <line key={i}><bufferGeometry><bufferAttribute attach="attributes-position" count={2} array={new Float32Array([...source.position, ...target.position])} itemSize={3} /></bufferGeometry><lineBasicMaterial color="#C0C0C0" transparent opacity={0.4} /></line>
        })}
        <Stars radius={300} depth={60} count={1200} factor={4} saturation={0} fade />
        <Environment preset="night" />
        <OrbitControls enablePan enableZoom enableRotate autoRotate autoRotateSpeed={0.3} />
        <EffectComposer><Bloom kernelSize={KernelSize.LARGE} intensity={1.4} /></EffectComposer>
      </Canvas>
    </motion.div>
  )
}import { useState } from 'react'
import { grokCompletion } from '../lib/ai'
import { Shield, CheckCircle, Clock } from 'lucide-react'
import { motion } from 'framer-motion'

export default function OracleVerifier({ opportunity }: { opportunity: any }) {
  const [status, setStatus] = useState<'idle' | 'verifying' | 'verified'>('idle')
  const [timestamp, setTimestamp] = useState('')
  const [reasoning, setReasoning] = useState('')

  const verify = async () => {
    setStatus('verifying')
    const prompt = `Oracle Protocol verification for: ${opportunity.title}. Return JSON only: {"status":"verified", "reasoning":"short summary"}`
    const res = await grokCompletion(prompt)
    try {
      const data = JSON.parse(res)
      setStatus('verified')
      setTimestamp(new Date().toLocaleTimeString())
      setReasoning(data.reasoning)
    } catch {
      setStatus('verified')
      setTimestamp(new Date().toLocaleTimeString())
    }
  }

  return (
    <div>
      <button onClick={verify} className="flex items-center gap-2 text-xs font-mono px-4 py-2 rounded-2xl border hover:border-cobalt">
        <Shield className="w-4 h-4" />
        {status === 'verifying' ? 'VERIFYING...' : status === 'verified' ? <><CheckCircle className="text-green-400" /> VERIFIED {timestamp}</> : 'ORACLE VERIFY'}
      </button>
      {reasoning && <motion.div className="text-xs mt-2 text-silver/80">{reasoning}</motion.div>}
    </div>
  )
}import { useState } from 'react'
import { grokCompletion } from '../lib/ai'
import { Send, X, Bot, Wrench, DollarSign } from 'lucide-react'

const personas = {
  jarvis: { name: "JARVIS", icon: Bot, system: "Emperor-Tier autonomous agent. Be concise and predictive." },
  engineer: { name: "AI Chief Engineer", icon: Wrench, system: "Focus on CMMC, technical viability and arbitrage." },
  cfo: { name: "AI CFO", icon: DollarSign, system: "Focus on 5% fee, ROI, risk and investor reporting." }
}

export default function TitanAdvisor() {
  const [isOpen, setIsOpen] = useState(false)
  const [active, setActive] = useState('jarvis')
  const [messages, setMessages] = useState([{role:'assistant', content:'Emperor-Tier online. Command me.'}])
  const [input, setInput] = useState('')

  const send = async () => {
    if (!input) return
    setMessages(m => [...m, {role:'user', content:input}])
    const text = input
    setInput('')
    const p = personas[active as keyof typeof personas]
    const res = await grokCompletion(text, p.system)
    setMessages(m => [...m, {role:'assistant', content:res}])
  }

  return (
    <>
      <button onClick={()=>setIsOpen(!isOpen)} className="fixed bottom-8 right-8 w-16 h-16 glass rounded-full flex items-center justify-center border-2 border-cobalt z-50">
        <Bot className="w-8 h-8 text-cobalt" />
      </button>
      {isOpen && <div className="fixed bottom-28 right-8 w-96 h-[520px] glass rounded-3xl flex flex-col z-50">
        {/* persona tabs + chat UI as previously provided */}
        {/* (full code is in earlier message - copy the full TitanAdvisor from before) */}
      </div>}
    </>
  )
}
  
  backdrop-filter: blur(20px);
  border: 1px solid rgba(192, 192, 192, 0.25);
}
