import React, { useState, useEffect, useRef } from 'react';
import { createRoot } from 'react-dom/client';
import { motion, AnimatePresence } from 'framer-motion';
import { 
  Github, Linkedin, Mail, Instagram, 
  Terminal, Cpu, Brain, Shield, 
  ChevronDown, ChevronUp, Code, 
  Cloud, Zap, Layers, Server, 
  Globe, Award, Smartphone, Database,
  ExternalLink
} from 'lucide-react';

// --- Background Animation Component ---
// Simulates a 3D fluid/orb environment using HTML5 Canvas
const BackgroundAnimation = () => {
  const canvasRef = useRef(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    let width, height;
    let particles = [];

    const init = () => {
      width = window.innerWidth;
      height = window.innerHeight;
      canvas.width = width;
      canvas.height = height;

      // Vibrant, deep colors
      const colors = [
        '#4F46E5', // Indigo
        '#7C3AED', // Violet
        '#EC4899', // Pink
        '#06B6D4', // Cyan
        '#10B981', // Emerald
      ];

      particles = Array.from({ length: 15 }, () => ({
        x: Math.random() * width,
        y: Math.random() * height,
        radius: Math.random() * 200 + 100,
        color: colors[Math.floor(Math.random() * colors.length)],
        vx: (Math.random() - 0.5) * 0.5,
        vy: (Math.random() - 0.5) * 0.5,
        blur: Math.random() * 20 + 20
      }));
    };

    const animate = () => {
      ctx.fillStyle = '#0f172a'; // Deep slate background
      ctx.fillRect(0, 0, width, height);

      particles.forEach(p => {
        p.x += p.vx;
        p.y += p.vy;

        // Bounce off edges
        if (p.x < -p.radius) p.x = width + p.radius;
        if (p.x > width + p.radius) p.x = -p.radius;
        if (p.y < -p.radius) p.y = height + p.radius;
        if (p.y > height + p.radius) p.y = -p.radius;

        // Draw glowing orb
        const gradient = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, p.radius);
        gradient.addColorStop(0, p.color + '40'); // 25% opacity
        gradient.addColorStop(1, 'transparent');

        ctx.fillStyle = gradient;
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fill();
      });

      requestAnimationFrame(animate);
    };

    init();
    animate();

    const handleResize = () => init();
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <canvas 
      ref={canvasRef} 
      className="fixed top-0 left-0 w-full h-full -z-10"
      style={{ filter: 'blur(30px)' }} // Extra blur for that smooth glass backdrop
    />
  );
};

// --- Shared UI Components ---

const GlassCard = ({ children, className = "", hoverEffect = false, onClick }) => (
  <motion.div
    whileHover={hoverEffect ? { scale: 1.02, backgroundColor: "rgba(255, 255, 255, 0.08)" } : {}}
    className={`backdrop-blur-xl bg-white/5 border border-white/10 rounded-2xl shadow-2xl overflow-hidden ${className}`}
    onClick={onClick}
  >
    {children}
  </motion.div>
);

const Badge = ({ text, color = "bg-indigo-500/20 text-indigo-300 border-indigo-500/30" }) => (
  <span className={`px-3 py-1 rounded-full text-xs font-medium border ${color} inline-block mr-2 mb-2`}>
    {text}
  </span>
);

const SectionTitle = ({ icon: Icon, title, subtitle }) => (
  <div className="flex flex-col items-center mb-12 text-center">
    <div className="p-3 bg-white/5 rounded-full mb-4 border border-white/10">
      <Icon className="w-8 h-8 text-indigo-400" />
    </div>
    <h2 className="text-3xl md:text-4xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400">
      {title}
    </h2>
    {subtitle && <p className="text-slate-400 mt-2 max-w-xl text-sm md:text-base">{subtitle}</p>}
  </div>
);

// --- Sections ---

const Header = () => {
  const [text, setText] = useState('');
  const fullText = "Systems Architect | AI Engineer | R&D";
  
  useEffect(() => {
    let index = 0;
    const timer = setInterval(() => {
      setText(fullText.slice(0, index + 1));
      index++;
      if (index > fullText.length) clearInterval(timer);
    }, 50);
    return () => clearInterval(timer);
  }, []);

  return (
    <header className="min-h-screen flex flex-col justify-center items-center text-center px-4 relative">
      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.8 }}
        className="z-10"
      >
        <h1 className="text-5xl md:text-7xl font-bold text-white mb-6 tracking-tight">
          Shaurya Pratap Singh
        </h1>
        <div className="h-8 mb-8">
          <p className="text-xl md:text-2xl text-indigo-300 font-mono">
            {text}<span className="animate-pulse">_</span>
          </p>
        </div>
        <p className="text-slate-400 max-w-2xl mx-auto mb-10 leading-relaxed">
          Creator of Nexus, Darshana & Panda. Engineering autonomous systems that push the boundaries of hybrid intelligence.
        </p>

        <div className="flex gap-4 justify-center">
          {[
            { Icon: Github, href: "https://github.com/ShauryaPratapSingh070408", label: "GitHub" },
            { Icon: Linkedin, href: "https://www.linkedin.com/in/shaurya-pratap-singh-244765335", label: "LinkedIn" },
            { Icon: Mail, href: "mailto:shaurya070408@gmail.com", label: "Email" },
            { Icon: Instagram, href: "https://www.instagram.com/shaurya.ai.dev", label: "Instagram" }
          ].map(({ Icon, href, label }, idx) => (
            <a 
              key={idx} 
              href={href} 
              target="_blank" 
              rel="noopener noreferrer"
              className="p-3 rounded-full bg-white/5 hover:bg-white/20 hover:scale-110 transition-all border border-white/10 text-white"
              title={label}
            >
              <Icon size={24} />
            </a>
          ))}
        </div>
      </motion.div>
      
      <motion.div 
        animate={{ y: [0, 10, 0] }}
        transition={{ repeat: Infinity, duration: 2 }}
        className="absolute bottom-10 text-slate-500"
      >
        <ChevronDown size={32} />
      </motion.div>
    </header>
  );
};

const ExecutiveProfile = () => (
  <section className="py-20 px-4 max-w-6xl mx-auto">
    <SectionTitle icon={Terminal} title="Executive Profile" />
    <GlassCard className="p-8 md:p-12 relative overflow-hidden">
      <div className="absolute top-0 right-0 -mr-16 -mt-16 w-64 h-64 bg-indigo-500/10 blur-3xl rounded-full pointer-events-none"></div>
      
      <div className="grid md:grid-cols-[auto_1fr] gap-8 items-start">
        <div className="flex justify-center md:block">
           <div className="w-20 h-20 rounded-2xl bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center shadow-lg">
             <Terminal className="text-white w-10 h-10" />
           </div>
        </div>
        <div className="text-slate-300 leading-relaxed space-y-4">
          <p>
            I am a Systems Architect and AI Engineer with a passion for engineering autonomous intelligence systems. My professional journey is rooted in creating self-sustaining digital ecosystems—environments where intelligent agents not only execute tasks but also autonomously debug, optimize, and evolve their own codebases in real-time.
          </p>
          <p>
            Operating at the intersection of edge computing, cloud-native orchestration, and advanced machine learning, I design solutions that seamlessly integrate on-device processing with distributed cloud resources. Whether it's architecting frameworks for recursive self-healing or pioneering multimodal reasoning pipelines, my work is driven by a commitment to reliability, efficiency, and ethical AI deployment.
          </p>
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mt-12">
        {[
          { icon: Cpu, title: "Hyper-Local Intelligence", sub: "Preserving Edge Execution", tag: "The Panda Protocol" },
          { icon: Brain, title: "Cognitive Orchestration", sub: "Strategic Planning", tag: "The Darshana Engine" },
          { icon: Shield, title: "Autonomous Reliability", sub: "Mission Critical Systems", tag: "The Nexus Framework" }
        ].map((item, idx) => (
          <div key={idx} className="bg-white/5 p-6 rounded-xl border border-white/5 hover:bg-white/10 transition-colors text-center">
            <item.icon className="w-8 h-8 text-indigo-400 mx-auto mb-4" />
            <h3 className="text-white font-semibold mb-1">{item.title}</h3>
            <p className="text-slate-400 text-xs mb-3">{item.sub}</p>
            <span className="text-xs font-mono text-purple-300 bg-purple-500/10 px-2 py-1 rounded">{item.tag}</span>
          </div>
        ))}
      </div>
    </GlassCard>
  </section>
);

const AccordionItem = ({ logo, title, role, location, tags, children }) => {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <GlassCard className="mb-6">
      <div 
        className="p-6 cursor-pointer flex flex-col md:flex-row items-start md:items-center justify-between gap-4"
        onClick={() => setIsOpen(!isOpen)}
      >
        <div className="flex items-center gap-4">
          <div className="p-3 bg-white/5 rounded-lg border border-white/10">
            {logo}
          </div>
          <div>
            <h3 className="text-xl font-bold text-white">{title}</h3>
            <p className="text-indigo-400 text-sm font-medium">{role}</p>
            <p className="text-slate-500 text-xs mt-1">{location}</p>
          </div>
        </div>
        <div className="flex items-center gap-4">
            <div className="hidden md:flex flex-wrap gap-2 justify-end">
                {tags.map((t, i) => <Badge key={i} text={t} />)}
            </div>
            <div className={`transition-transform duration-300 ${isOpen ? 'rotate-180' : ''}`}>
                <ChevronDown className="text-slate-400" />
            </div>
        </div>
      </div>
      
      <AnimatePresence>
        {isOpen && (
          <motion.div
            initial={{ height: 0, opacity: 0 }}
            animate={{ height: "auto", opacity: 1 }}
            exit={{ height: 0, opacity: 0 }}
            className="overflow-hidden"
          >
            <div className="p-6 pt-0 border-t border-white/10 text-slate-300 space-y-4 text-sm leading-relaxed">
              {children}
            </div>
          </motion.div>
        )}
      </AnimatePresence>
    </GlassCard>
  );
};

const Recognition = () => (
  <section className="py-20 px-4 max-w-5xl mx-auto">
    <SectionTitle icon={Award} title="Global Recognition" subtitle="Contributions to the world's leading AI infrastructures" />
    
    <AccordionItem 
      logo={<Cloud className="text-orange-500" />}
      title="Alibaba Cloud"
      role="Magic Developer (MVP)"
      location="Beijing HQ"
      tags={["LLM Optimization", "ModelScope", "Distributed Training"]}
    >
      <div className="space-y-4">
        <div className="bg-orange-500/10 p-4 rounded-lg border border-orange-500/20">
            <h4 className="text-orange-300 font-bold mb-2">Conference Delegate - Beijing</h4>
            <p>Invited as a distinguished contributor to the Magic Developer Conference. Collaborated with 200+ elite scholars on LLM architecture.</p>
        </div>
        <ul className="list-disc pl-5 space-y-2 text-slate-400">
            <li><strong>ModelScope Architecture:</strong> Contributed to efficient quantization techniques reducing inference latency by 40%.</li>
            <li><strong>Qwen2.5 Testing:</strong> Provided early access benchmarking (MMLU, HumanEval) contributing to model robustness.</li>
            <li><strong>Research:</strong> Engaged in federated learning and secure multi-party computation discussions.</li>
        </ul>
      </div>
    </AccordionItem>

    <AccordionItem 
      logo={<Globe className="text-purple-500" />}
      title="Moonshot AI"
      role="Research & Development"
      location="Kimi k1.5-Preview"
      tags={["Multimodal Logic", "RLHF", "Hallucination Analysis"]}
    >
      <div className="space-y-4">
        <div className="bg-purple-500/10 p-4 rounded-lg border border-purple-500/20">
            <h4 className="text-purple-300 font-bold mb-2">Pre-Release Model Testing</h4>
            <p>Gained privileged access to o1-level multimodal reasoning models. Direct feedback loops reduced hallucination rates by 15%.</p>
        </div>
        <div className="grid grid-cols-2 gap-4 text-xs">
            <div className="bg-white/5 p-3 rounded"><strong>MATH-500 Score:</strong> 94.6</div>
            <div className="bg-white/5 p-3 rounded"><strong>Context Window:</strong> 200K+ Tokens</div>
        </div>
        <p className="text-slate-400">Contributed novel techniques for RLHF in multilingual settings and led evaluations for long-context reasoning.</p>
      </div>
    </AccordionItem>

    <AccordionItem 
      logo={<Database className="text-blue-500" />}
      title="Google Developers"
      role="Certified Professional"
      location="Verified Expert"
      tags={["Cloud Innovator", "Firebase Studio", "Vertex AI"]}
    >
      <div className="space-y-4">
         <div className="flex flex-wrap gap-2 mb-4">
             <Badge text="NVIDIA x Google Cloud Medal" color="bg-green-500/20 text-green-300 border-green-500/30" />
             <Badge text="Code Wiki Contributor" color="bg-blue-500/20 text-blue-300 border-blue-500/30" />
             <Badge text="Firebase Studio Dev" color="bg-yellow-500/20 text-yellow-300 border-yellow-500/30" />
         </div>
         <p className="text-slate-400">
             Expertise in Cloud Run for serverless microservices and Compute Engine for GPU-accelerated ML training. 
             Awarded for pioneering GPU infrastructure optimizations including kernel-level CUDA tuning.
         </p>
      </div>
    </AccordionItem>
  </section>
);

const IntelligenceEcosystem = () => {
    const [activeTab, setActiveTab] = useState('nexus');

    const systems = {
        nexus: {
            title: "Nexus",
            tagline: "Gen-9 Autonomous Intelligence",
            color: "text-emerald-400",
            borderColor: "border-emerald-500/50",
            icon: Shield,
            desc: "Flagship system engineered for granular OS-level control. Nexus proactively models workflows, allocates resources, and iterates on performance without human oversight.",
            features: [
                { name: "ReForge Tier 1", desc: "Advanced self-healing. Deep stack trace analysis & hot-reloading patches." },
                { name: "Dynamic Restoration", desc: "Predictive analytics to forecast failures before they happen." },
                { name: "DreamForge Engine", desc: "Generative foresight module for creative problem solving." }
            ]
        },
        darshana: {
            title: "Darshana",
            tagline: "Gen-6 Cloud Cognitive AI",
            color: "text-blue-400",
            borderColor: "border-blue-500/50",
            icon: Brain,
            desc: "The intellectual nexus. Darshana acts as the Cognitive Bridge deciphering nuanced human intent and ensuring coherent knowledge propagation across distributed systems.",
            features: [
                { name: "ReForge Tier 2", desc: "Distributed recovery for cloud-scale ops and API throttling management." },
                { name: "Contextual Continuity", desc: "Vector embeddings graph for sessions spanning weeks." },
                { name: "Nuance Analysis", desc: "Sentiment and cultural context modeling." }
            ]
        },
        panda: {
            title: "Panda",
            tagline: "Gen-5 Lightweight Intelligence",
            color: "text-amber-400",
            borderColor: "border-amber-500/50",
            icon: Smartphone,
            desc: "Efficiency at the edge. An ultra-lean execution engine optimized for mobile devices and IoT nodes, prioritizing offline autonomy and privacy.",
            features: [
                { name: "ReForge Tier 3", desc: "Battery-aware restarts and permission auto-grants." },
                { name: "Intent Injection", desc: "Direct interface with Android ActivityManager." },
                { name: "Gateway Protocol", desc: "Intelligent routing of complex tasks to cloud tiers." }
            ]
        }
    };

    const ActiveIcon = systems[activeTab].icon;

    return (
        <section className="py-20 px-4 max-w-6xl mx-auto">
            <SectionTitle icon={Layers} title="Intelligence Ecosystem" subtitle="Unified Dynamic Restoration with progressive capability escalation" />
            
            <div className="flex justify-center gap-4 mb-8">
                {Object.keys(systems).map((key) => (
                    <button
                        key={key}
                        onClick={() => setActiveTab(key)}
                        className={`px-6 py-2 rounded-full font-medium transition-all duration-300 border ${activeTab === key ? 'bg-white/10 border-white/40 text-white' : 'bg-transparent border-transparent text-slate-500 hover:text-slate-300'}`}
                    >
                        {systems[key].title}
                    </button>
                ))}
            </div>

            <GlassCard className="p-8 md:p-12 min-h-[400px]">
                <div className="grid lg:grid-cols-2 gap-12 items-center h-full">
                    <motion.div 
                        key={activeTab}
                        initial={{ opacity: 0, x: -20 }}
                        animate={{ opacity: 1, x: 0 }}
                        transition={{ duration: 0.5 }}
                    >
                        <div className={`inline-flex items-center gap-2 px-3 py-1 rounded border ${systems[activeTab].borderColor} ${systems[activeTab].color} bg-white/5 text-sm mb-6`}>
                            <ActiveIcon size={16} />
                            {systems[activeTab].tagline}
                        </div>
                        <h3 className="text-4xl font-bold text-white mb-6">{systems[activeTab].title}</h3>
                        <p className="text-slate-300 text-lg leading-relaxed mb-8">
                            {systems[activeTab].desc}
                        </p>
                    </motion.div>

                    <motion.div 
                        key={`${activeTab}-features`}
                        initial={{ opacity: 0, x: 20 }}
                        animate={{ opacity: 1, x: 0 }}
                        transition={{ duration: 0.5, delay: 0.1 }}
                        className="space-y-4"
                    >
                        {systems[activeTab].features.map((feat, idx) => (
                            <div key={idx} className="bg-white/5 border border-white/5 p-4 rounded-xl hover:bg-white/10 transition-colors">
                                <h4 className={`font-bold ${systems[activeTab].color} mb-1`}>{feat.name}</h4>
                                <p className="text-slate-400 text-sm">{feat.desc}</p>
                            </div>
                        ))}
                    </motion.div>
                </div>
            </GlassCard>
        </section>
    );
};

const ProjectCard = ({ title, desc, tags, stats }) => (
    <GlassCard hoverEffect={true} className="flex flex-col h-full p-6">
        <div className="mb-4">
            <h3 className="text-xl font-bold text-white mb-2">{title}</h3>
            <div className="flex flex-wrap gap-2 mb-4">
                {tags.map((tag, i) => (
                    <span key={i} className="text-[10px] uppercase tracking-wider text-slate-400 bg-white/5 px-2 py-1 rounded">
                        {tag}
                    </span>
                ))}
            </div>
        </div>
        <p className="text-slate-300 text-sm mb-6 flex-grow leading-relaxed">
            {desc}
        </p>
        <div className="pt-4 border-t border-white/10 text-xs text-slate-500 font-mono">
            {stats}
        </div>
    </GlassCard>
);

const Projects = () => (
    <section className="py-20 px-4 max-w-6xl mx-auto">
        <SectionTitle icon={Code} title="Featured Projects" />
        <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
            <ProjectCard 
                title="Nexus Framework" 
                tags={["Gen-9", "Python", "Local NPU", "CUDA"]}
                desc="A groundbreaking multi-generational AI orchestrator that achieves true autonomy through predictive planning and self-evolution. Includes voice command and full-stack autonomous coding."
                stats="15K+ LoC • 50+ Contributors • 99.8% Recovery"
            />
            <ProjectCard 
                title="Darshana Cloud" 
                tags={["Gen-6", "TypeScript", "Serverless", "Vector DB"]}
                desc="Scalable cloud-native platform for advanced cognition. Excels in handling ambiguous queries and facilitating multi-agent collaborations via the ReForge Engine."
                stats="1M+ Daily Inferences • Open Source Apache 2.0"
            />
            <ProjectCard 
                title="Panda Mobile" 
                tags={["Gen-5", "Kotlin", "TFLite", "Offline ML"]}
                desc="Featherweight on-device AI executor. Powers offline-first experiences and secure gateways to higher tiers with minimal battery impact."
                stats="5K+ Downloads • <2% Battery Drain • MIT License"
            />
             <ProjectCard 
                title="CloudWorker Engine" 
                tags={["NodeJS", "Go", "gRPC", "Redis"]}
                desc="Robust engine for managing microservices in high-throughput environments. Features built-in LLM chaining and adaptive scaling based on queue depth."
                stats="10M+ Events/Day • AGPL-3.0"
            />
            <ProjectCard 
                title="LLM Multimodel Orch" 
                tags={["Next.js", "LangChain", "Python"]}
                desc="Intelligent middleware democratizing access to diverse LLMs. Evaluates query semantics to route to optimal providers for cost/accuracy balance."
                stats="2K+ Stars • 25% Avg Cost Savings"
            />
        </div>
    </section>
);

const TechStack = () => (
    <section className="py-20 px-4 max-w-4xl mx-auto text-center">
        <h3 className="text-2xl font-bold text-white mb-8">Technical Arsenal</h3>
        <GlassCard className="p-8">
            <div className="space-y-8">
                <div>
                    <h4 className="text-sm uppercase tracking-widest text-slate-500 mb-4">Languages</h4>
                    <div className="flex flex-wrap justify-center gap-3">
                        {['Python', 'TypeScript', 'Go', 'Dart', 'Kotlin', 'C++'].map(t => <Badge key={t} text={t} color="bg-blue-500/10 text-blue-300 border-blue-500/20" />)}
                    </div>
                </div>
                <div>
                    <h4 className="text-sm uppercase tracking-widest text-slate-500 mb-4">Frameworks & Core</h4>
                    <div className="flex flex-wrap justify-center gap-3">
                        {['Next.js', 'Flutter', 'React', 'Node.js', 'TensorFlow', 'PyTorch'].map(t => <Badge key={t} text={t} color="bg-purple-500/10 text-purple-300 border-purple-500/20" />)}
                    </div>
                </div>
                <div>
                    <h4 className="text-sm uppercase tracking-widest text-slate-500 mb-4">Infrastructure</h4>
                    <div className="flex flex-wrap justify-center gap-3">
                        {['Google Cloud', 'Docker', 'Kubernetes', 'Firebase', 'CUDA', 'Redis'].map(t => <Badge key={t} text={t} color="bg-emerald-500/10 text-emerald-300 border-emerald-500/20" />)}
                    </div>
                </div>
            </div>
        </GlassCard>
    </section>
);

const Footer = () => (
    <footer className="py-12 text-center text-slate-500 text-sm relative z-10 bg-black/20 backdrop-blur-md">
        <div className="max-w-4xl mx-auto px-4">
            <p className="mb-4">Built by Shaurya Pratap Singh | Pioneering the future of autonomous intelligence</p>
            <div className="flex justify-center gap-4 mb-8">
                <a href="mailto:shaurya070408@gmail.com" className="hover:text-white transition-colors">Contact</a>
                <span>•</span>
                <a href="https://github.com/ShauryaPratapSingh070408" className="hover:text-white transition-colors">GitHub</a>
                <span>•</span>
                <a href="https://www.linkedin.com/in/shaurya-pratap-singh-244765335" className="hover:text-white transition-colors">LinkedIn</a>
            </div>
            <p className="font-mono text-xs opacity-50">01000100 01110010 01101001 01110011 01101000 01110100 01101001</p>
        </div>
    </footer>
);

const App = () => {
    return (
        <div className="min-h-screen text-slate-200 font-sans selection:bg-indigo-500/30">
            <BackgroundAnimation />
            
            <main className="relative z-10">
                <Header />
                <ExecutiveProfile />
                <Recognition />
                <IntelligenceEcosystem />
                <Projects />
                <TechStack />
                <div className="py-20 px-4 text-center">
                    <GlassCard className="inline-block p-8 hover:bg-white/10 transition-colors cursor-pointer" onClick={() => window.location.href='mailto:shaurya070408@gmail.com'}>
                        <h3 className="text-2xl font-bold text-white mb-2">Ready to Collaborate?</h3>
                        <p className="text-indigo-300 mb-4">Open for AI Architecture & Cloud Systems Research</p>
                        <div className="inline-flex items-center gap-2 text-white bg-indigo-600 hover:bg-indigo-700 px-6 py-2 rounded-full transition-colors">
                            <Mail size={18} /> Let's Connect
                        </div>
                    </GlassCard>
                </div>
            </main>
            
            <Footer />
        </div>
    );
};

const root = createRoot(document.getElementById('root'));
root.render(<App />);

