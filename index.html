import React, { useEffect, useRef, useState } from 'react';
import * as THREE from 'three';

// Native scroll tracking to replace external dependencies
const SignalGraph = {
    scrollProgress: 0,
    update() {
        const doc = document.documentElement;
        const scrollTotal = doc.scrollHeight - doc.clientHeight;
        if (scrollTotal > 0) {
            this.scrollProgress = doc.scrollTop / scrollTotal;
        }
    }
};

const ShaderBackground = () => {
    const mountRef = useRef(null);

    useEffect(() => {
        if (!mountRef.current) return;

        const scene = new THREE.Scene();
        const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);
        const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: false });
        
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        mountRef.current.appendChild(renderer.domElement);

        const uniforms = {
            uTime: { value: 0 },
            uProgress: { value: 0 },
            uResolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) }
        };

        const material = new THREE.ShaderMaterial({
            uniforms,
            depthWrite: false,
            depthTest: false,
            vertexShader: `
                varying vec2 vUv;
                void main() {
                    vUv = uv;
                    gl_Position = vec4(position.xy, 0.0, 1.0);
                }
            `,
            fragmentShader: `
                varying vec2 vUv;
                uniform float uTime;
                uniform float uProgress;
                uniform vec2 uResolution;

                #define MAX_STEPS 80
                #define MAX_DIST 50.0
                #define SURF_DIST 0.005

                mat2 rot(float a) {
                    float s = sin(a), c = cos(a);
                    return mat2(c, -s, s, c);
                }

                float sdBox(vec3 p, vec3 b) {
                    vec3 q = abs(p) - b;
                    return length(max(q,0.0)) + min(max(q.x,max(q.y,q.z)),0.0);
                }

                float getDist(vec3 p) {
                    vec3 bp = p;
                    bp.y -= 0.5;
                    bp.xy *= rot(uTime * 0.2);
                    bp.xz *= rot(uTime * 0.15);

                    float noise = sin(p.x * 2.5 + uTime * 1.5) * sin(p.y * 2.5 + uTime * 1.2) * sin(p.z * 2.5) * 0.3;
                    float desert = length(p) - 1.2 + noise * (1.0 - uProgress);
                    float box = sdBox(bp, vec3(0.8, 0.8, 0.8));

                    float morph = smoothstep(0.2, 0.8, uProgress);
                    return mix(desert, box, morph);
                }

                vec3 getNormal(vec3 p) {
                    float d = getDist(p);
                    vec2 e = vec2(0.01, 0.0);
                    vec3 n = d - vec3(
                        getDist(p - e.xyy),
                        getDist(p - e.yxy),
                        getDist(p - e.yyx)
                    );
                    return normalize(n);
                }

                void main() {
                    vec2 uv = (vUv - 0.5) * 2.0;
                    uv.x *= uResolution.x / uResolution.y;

                    // Camera interpolation
                    vec3 ro = mix(
                        vec3(0.0, 0.0, 4.0), 
                        vec3(2.0, 1.5, 3.0), 
                        smoothstep(0.0, 1.0, uProgress)
                    );
                    
                    vec3 target = vec3(0.0, 0.0, 0.0);
                    vec3 f = normalize(target - ro);
                    vec3 r = normalize(cross(vec3(0.0, 1.0, 0.0), f));
                    vec3 u = cross(f, r);
                    vec3 rd = normalize(uv.x * r + uv.y * u + 1.5 * f);

                    float d0 = 0.0;
                    for(int i=0; i<MAX_STEPS; i++) {
                        vec3 p = ro + rd * d0;
                        float dS = getDist(p);
                        d0 += dS;
                        if(d0 > MAX_DIST || abs(dS) < SURF_DIST) break;
                    }

                    // Dynamically synthesized palettes matching the HTML
                    vec3 bgDesert = vec3(0.97, 0.94, 0.89); // #F8F1E3
                    vec3 bgSanc = vec3(0.02, 0.02, 0.02);   // #050505
                    vec3 bg = mix(bgDesert, bgSanc, uProgress);
                    vec3 col = bg;

                    if(d0 < MAX_DIST) {
                        vec3 p = ro + rd * d0;
                        vec3 n = getNormal(p);
                        
                        vec3 lightPos = mix(vec3(2.0, 4.0, 3.0), vec3(-2.0, 4.0, -3.0), uProgress);
                        vec3 l = normalize(lightPos - p);
                        float diff = max(dot(n, l), 0.0);
                        
                        vec3 desertLight = vec3(0.8, 0.6, 0.4); 
                        vec3 desertDark = vec3(0.4, 0.3, 0.2);
                        vec3 sancLight = vec3(0.08, 0.72, 0.65); // #14B8A6
                        vec3 sancDark = vec3(0.04, 0.11, 0.10);

                        float morph = smoothstep(0.2, 0.8, uProgress);
                        vec3 matLight = mix(desertLight, sancLight, morph);
                        vec3 matDark = mix(desertDark, sancDark, morph);

                        col = mix(matDark, matLight, diff);

                        float fresnel = pow(1.0 - max(dot(n, -rd), 0.0), 4.0);
                        col += fresnel * matLight * 0.8;
                        
                        float ao = clamp(p.y * 0.5 + 0.5, 0.0, 1.0);
                        col *= ao;

                        col = mix(col, bg, 1.0 - exp(-0.02 * d0 * d0));
                    }

                    // Film grain noise
                    float noise = fract(sin(dot(uv, vec2(12.9898, 78.233))) * 43758.5453);
                    col += noise * (mix(0.04, 0.015, uProgress));

                    col = col / (col + vec3(1.0));
                    col = pow(col, vec3(1.0/2.2));

                    gl_FragColor = vec4(col, 1.0);
                }
            `
        });

        const geometry = new THREE.PlaneGeometry(2, 2);
        const mesh = new THREE.Mesh(geometry, material);
        scene.add(mesh);

        const clock = new THREE.Clock();
        let animationFrameId;

        const animate = () => {
            animationFrameId = requestAnimationFrame(animate);
            uniforms.uTime.value = clock.getElapsedTime();
            const targetProgress = SignalGraph.scrollProgress;
            uniforms.uProgress.value += (targetProgress - uniforms.uProgress.value) * 0.08;
            renderer.render(scene, camera);
        };
        animate();

        const handleResize = () => {
            const width = window.innerWidth;
            const height = window.innerHeight;
            renderer.setSize(width, height);
            uniforms.uResolution.value.set(width, height);
        };
        window.addEventListener('resize', handleResize);

        return () => {
            window.removeEventListener('resize', handleResize);
            cancelAnimationFrame(animationFrameId);
            if (mountRef.current) {
                mountRef.current.removeChild(renderer.domElement);
            }
            geometry.dispose();
            material.dispose();
            renderer.dispose();
        };
    }, []);

    return <div ref={mountRef} className="w-full h-full" />;
};

// Intersection Observer based kinetic reveal
const KineticText = ({ children, className, delay = 0, tag: Tag = 'span' }) => {
    const [isVisible, setIsVisible] = useState(false);
    const ref = useRef();

    useEffect(() => {
        const observer = new IntersectionObserver(([entry]) => {
            if (entry.isIntersecting) {
                setIsVisible(true);
            }
        }, { threshold: 0.1 });
        
        if (ref.current) observer.observe(ref.current);
        return () => observer.disconnect();
    }, []);

    const style = {
        transform: isVisible ? 'translateY(0) scale(1)' : 'translateY(20px) scale(0.98)',
        opacity: isVisible ? 1 : 0,
        transition: `transform 0.8s cubic-bezier(0.23, 1, 0.32, 1) ${delay}s, opacity 0.8s cubic-bezier(0.23, 1, 0.32, 1) ${delay}s`,
        willChange: 'transform, opacity'
    };

    return (
        <Tag ref={ref} className={`block ${className}`} style={style}>
            {children}
        </Tag>
    );
};

export default function App() {
    const [nights, setNights] = useState(3);
    const [arrival, setArrival] = useState('2026-07-12');
    const [departure, setDeparture] = useState('2026-07-15');
    const [keyEngaged, setKeyEngaged] = useState(false);
    const [showModal, setShowModal] = useState(false);

    useEffect(() => {
        const handleScroll = () => SignalGraph.update();
        window.addEventListener('scroll', handleScroll, { passive: true });
        handleScroll(); // Init
        return () => window.removeEventListener('scroll', handleScroll);
    }, []);

    useEffect(() => {
        const start = new Date(arrival);
        const end = new Date(departure);
        if (start && end && end > start) {
            setNights(Math.ceil((end - start) / (1000 * 60 * 60 * 24)));
        } else {
            setNights(0);
        }
    }, [arrival, departure]);

    const handleKeyTurn = () => {
        setKeyEngaged(true);
        setTimeout(() => setShowModal(true), 1200);
    };

    return (
        <div className="relative w-full overflow-hidden bg-[#050505] text-[#F1F1F1] selection:bg-[#14B8A6] selection:text-black font-sans">
            <style dangerouslySetInnerHTML={{__html: `
                @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;900&display=swap');
                
                :root { --font-primary: 'Inter', system-ui, sans-serif; }
                body { 
                    font-family: var(--font-primary); 
                    margin: 0; padding: 0; 
                    background: #050505; 
                    -webkit-font-smoothing: antialiased;
                }
                
                .premium-shadow {
                    box-shadow: 0 10px 30px -15px rgb(0 0 0 / 0.6), inset 0 1px 0 rgb(255 255 255 / 0.04);
                }

                .concierge-input {
                    background: transparent;
                    border: none;
                    border-bottom: 1px solid #2A2A2A;
                    color: #F1F1F1;
                    font-feature-settings: "kern" "tnum";
                }
                .concierge-input:focus {
                    outline: none;
                    border-bottom-color: #14B8A6;
                }
                
                .key-svg { filter: drop-shadow(0 30px 35px rgb(0 0 0 / 0.55)); transition: transform 680ms cubic-bezier(0.23, 1, 0.32, 1); }
                .key-turn { transform: rotate(90deg); }
            `}} />

            {/* Depth mechanics: Layered Z-index architecture integrating fragment shaders */}
            <div className="fixed inset-0 z-0 pointer-events-none mix-blend-screen opacity-60">
                <ShaderBackground />
            </div>

            <div className="relative z-10 w-full flex flex-col">
                
                {/* STAGE 1: DESERT EXPOSURE */}
                <section className="min-h-screen w-full flex flex-col justify-center px-6 md:px-16 lg:px-32 relative">
                    <div className="absolute top-9 left-9 z-20 flex items-center gap-x-2.5">
                        <div className="h-px w-5 bg-black/30"></div>
                        <span className="text-[10px] text-black/50 tracking-[3px] font-semibold uppercase">Phoenix</span>
                    </div>

                    <div className="max-w-[980px] mx-auto text-center relative z-10">
                        <KineticText className="text-[118px] md:text-[138px] font-black leading-none text-[#1F1A17] tracking-[-0.06em] mb-5">
                            115°
                        </KineticText>
                        <div className="max-w-[420px] mx-auto">
                            <KineticText delay={0.1} className="font-black text-[23px] md:text-[26px] text-[#1F1A17] tracking-[-0.035em] leading-none mb-3">
                                THE HEAT DOES NOT NEGOTIATE.
                            </KineticText>
                            <KineticText delay={0.2} className="text-[#3F2E26] text-[15px] font-medium tracking-[-0.01em]">
                                There is no shade. No shelter. Only exposure.
                            </KineticText>
                        </div>
                    </div>

                    <div className="absolute bottom-14 left-0 right-0 px-6 z-20 text-center">
                        <div className="mt-5 text-[10px] text-[#3F2E26]/60 tracking-[2px] font-medium">SCROLL TO ENTER</div>
                    </div>
                </section>

                {/* STAGE 2: SANCTUARY */}
                <section className="min-h-screen w-full flex flex-col px-8 pt-24 pb-8 max-w-[1080px] mx-auto relative">
                    <div className="flex items-center justify-between mb-10">
                        <KineticText delay={0} className="font-black text-4xl tracking-[-0.04em] text-white">THE VEIL</KineticText>
                        <div className="px-3 py-1 border border-white/10 rounded text-[10px] font-medium tracking-[2px] text-[#14B8A6]">SEALED</div>
                    </div>

                    <div className="mb-16">
                        <div className="space-y-[-2px]">
                            <KineticText delay={0.1} className="font-light text-[58px] md:text-[72px] leading-[0.9] text-white tracking-[-0.035em]">Ice cold air.</KineticText>
                            <KineticText delay={0.15} className="font-light text-[58px] md:text-[72px] leading-[0.9] text-white/90 tracking-[-0.035em]">Memory foam.</KineticText>
                            <KineticText delay={0.2} className="font-light text-[58px] md:text-[72px] leading-[0.9] text-white/75 tracking-[-0.035em]">No street noise.</KineticText>
                        </div>
                        <KineticText delay={0.3} className="mt-6 text-xs text-[#14B8A6] font-semibold tracking-[2.5px] uppercase">
                            68° CONSTANT • ZERO VIBRATION
                        </KineticText>
                    </div>

                    <div className="grid grid-cols-1 md:grid-cols-3 gap-px bg-white/5 mb-16">
                        <div className="bg-[#050505] p-7 premium-shadow">
                            <div className="text-[10px] text-[#14B8A6] font-semibold mb-4 tracking-[2px]">TEMPERATURE</div>
                            <div className="font-black text-[42px] leading-none tracking-[-0.04em] text-white">68°<span className="text-2xl align-super text-white/40">F</span></div>
                            <div className="mt-4 text-[13.5px] text-white/70 tracking-[-0.011em] leading-relaxed">The air meets your skin before your eyes adjust. Your shoulders drop 14 millimeters.</div>
                        </div>
                        <div className="bg-[#050505] p-7 premium-shadow">
                            <div className="text-[10px] text-[#14B8A6] font-semibold mb-4 tracking-[2px]">SOUND</div>
                            <div className="font-black text-[42px] leading-none tracking-[-0.04em] text-white">0<span className="text-2xl align-super text-white/40">dB</span></div>
                            <div className="mt-4 text-[13.5px] text-white/70 tracking-[-0.011em] leading-relaxed">400mm concrete. No neighbors above. No neighbors below. Your pulse is the loudest sound.</div>
                        </div>
                        <div className="bg-[#050505] p-7 premium-shadow">
                            <div className="text-[10px] text-[#14B8A6] font-semibold mb-4 tracking-[2px]">LIGHT</div>
                            <div className="font-black text-[42px] leading-none tracking-[-0.04em] text-white">ZERO</div>
                            <div className="mt-4 text-[13.5px] text-white/70 tracking-[-0.011em] leading-relaxed">Full blackout. One controlled slit. You decide when the desert is allowed back in.</div>
                        </div>
                    </div>
                </section>

                {/* STAGE 3: THE KEY */}
                <section className="min-h-screen w-full flex flex-col px-8 pt-24 pb-6 max-w-[1100px] mx-auto relative border-t border-white/10">
                    <div className="flex justify-between items-start mb-16">
                        <div>
                            <span className="font-black text-[42px] tracking-[-0.04em]">THE KEY</span>
                            <p className="text-[#14B8A6] text-xs font-semibold tracking-[2.5px] mt-1">HAND THIS TO THE CONCIERGE</p>
                        </div>
                    </div>

                    <div className="flex flex-col lg:flex-row gap-x-14 gap-y-16 items-start">
                        {/* Premium Brass Key */}
                        <div className="flex-shrink-0 pt-2 w-full lg:w-auto flex flex-col items-center">
                            <div className="relative">
                                <svg width="255" height="335" viewBox="0 0 255 335" fill="none" className={`key-svg ${keyEngaged ? 'key-turn' : ''}`}>
                                    <path d="M82 292 Q 90 320 120 320 Q 150 320 158 292" fill="#000000" opacity="0.4"/>
                                    <rect x="103" y="92" width="24" height="198" rx="2" fill="#B8860B"/>
                                    <rect x="105" y="92" width="20" height="198" rx="1" fill="#D4A017"/>
                                    <circle cx="115" cy="68" r="42" fill="#B8860B"/>
                                    <circle cx="115" cy="68" r="34" fill="#D4A017"/>
                                    <circle cx="115" cy="68" r="25" fill="#8B6914"/>
                                    <circle cx="115" cy="68" r="15" fill="#B8860B"/>
                                    <circle cx="115" cy="68" r="8.5" fill="#2C2522"/>
                                    <rect x="103" y="92" width="3.5" height="198" fill="#E8C56B" opacity="0.28"/>
                                    <circle cx="115" cy="68" r="42" fill="none" stroke="#E8C56B" strokeWidth="1.25" opacity="0.2"/>
                                    <rect x="103" y="275" width="24" height="15" fill="#8B6914"/>
                                    <rect x="103" y="290" width="11" height="12" fill="#8B6914"/>
                                    <rect x="115" y="290" width="12" height="12" fill="#8B6914"/>
                                    <text x="115" y="74" fontFamily="Inter, sans-serif" fontSize="9.5" fill="#2C2522" textAnchor="middle" fontWeight="700">472</text>
                                </svg>
                                <div className="text-center mt-6">
                                    <span className="font-medium text-xs tracking-[2.5px] text-[#14B8A6]/70 uppercase">
                                        {keyEngaged ? 'SEALED • HOLDING' : 'HEAVY • PRECISION • YOURS'}
                                    </span>
                                </div>
                            </div>
                        </div>

                        <div className="flex-1 max-w-[460px] pt-4">
                            <div className="mb-10">
                                <div className="text-xs text-[#14B8A6] font-semibold tracking-[2.5px] mb-1.5 uppercase">THE RITUAL</div>
                                <div className="font-black text-[29px] tracking-[-0.03em] leading-none text-white">Hand the key.<br/>Receive the seal.</div>
                            </div>

                            <div className="space-y-10">
                                <div>
                                    <div className="text-[10px] font-semibold text-white/50 tracking-[2px] mb-4 uppercase">WHEN DO YOU NEED RELIEF?</div>
                                    <div className="grid grid-cols-2 gap-8">
                                        <div>
                                            <div className="text-xs text-white/40 mb-2 font-medium tracking-wider">ARRIVAL</div>
                                            <input type="date" value={arrival} onChange={e => setArrival(e.target.value)} className="concierge-input w-full text-[17px] py-1" />
                                        </div>
                                        <div>
                                            <div className="text-xs text-white/40 mb-2 font-medium tracking-wider">DEPARTURE</div>
                                            <input type="date" value={departure} onChange={e => setDeparture(e.target.value)} className="concierge-input w-full text-[17px] py-1" />
                                        </div>
                                    </div>
                                </div>

                                <div>
                                    <div className="text-[10px] font-semibold text-white/50 tracking-[2px] mb-4 uppercase">WHO RECEIVES THE KEY?</div>
                                    <input type="text" placeholder="YOUR NAME" defaultValue="Cecy" className="concierge-input w-full text-[19px] py-2" />
                                </div>

                                <div className="flex items-baseline gap-x-3 pt-2">
                                    <div>
                                        <span className="font-black text-[56px] tracking-[-0.045em] leading-none">{nights > 0 ? nights : '—'}</span>
                                        <span className="font-medium text-xs text-white/50 tracking-[1.5px] ml-1.5 uppercase">NIGHTS</span>
                                    </div>
                                    <div className="text-xs text-[#14B8A6]/70 font-medium tracking-wider uppercase ml-auto">ONE BODY • ONE KEY</div>
                                </div>

                                <div className="pt-8 border-t border-white/10">
                                    <button 
                                        onClick={handleKeyTurn}
                                        disabled={keyEngaged}
                                        className="w-full flex items-center justify-center gap-x-3 bg-white hover:bg-[#14B8A6] active:bg-[#0F9A8A] text-black py-5 font-bold tracking-[2px] text-xs uppercase transition-all disabled:opacity-50 disabled:cursor-not-allowed">
                                        <span>{keyEngaged ? 'ENGAGING...' : 'TURN THE KEY'}</span>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>
            </div>

            {/* Confirmation Modal */}
            {showModal && (
                <div className="fixed inset-0 z-50 bg-black/95 flex items-center justify-center p-6 transition-opacity animate-in fade-in duration-300">
                    <div className="max-w-md w-full bg-[#050505] border border-white/10 p-9 premium-shadow relative">
                        <div className="text-center">
                            <div className="mx-auto w-8 h-px bg-[#14B8A6] mb-6"></div>
                            
                            <div className="font-black text-5xl tracking-[-0.04em] mb-px">THE SEAL</div>
                            <div className="text-[#14B8A6] text-xs font-semibold tracking-[3px]">IS ENGAGED</div>
                            
                            <div className="my-9 space-y-px text-left max-w-[270px] mx-auto text-sm">
                                <div className="flex justify-between py-[9px] border-b border-white/10">
                                    <span className="text-white/50">KEY</span>
                                    <span className="font-medium tracking-wider">#472</span>
                                </div>
                                <div className="flex justify-between py-[9px] border-b border-white/10">
                                    <span className="text-white/50">ARRIVAL</span>
                                    <span className="font-mono text-xs">{arrival}</span>
                                </div>
                                <div className="flex justify-between py-[9px] border-b border-white/10">
                                    <span className="text-white/50">DEPARTURE</span>
                                    <span className="font-mono text-xs">{departure}</span>
                                </div>
                                <div className="flex justify-between py-[9px]">
                                    <span className="text-white/50">NIGHTS</span>
                                    <span className="font-medium">{nights}</span>
                                </div>
                            </div>
                            
                            <div className="text-xs text-white/55 leading-tight mb-8">
                                The door has sealed behind you.<br/>
                                68° is now the only temperature that exists.
                            </div>
                            
                            <button onClick={() => setShowModal(false)} className="w-full py-4 text-xs font-semibold tracking-[2px] border border-white/30 hover:bg-white hover:text-black transition-all">
                                RETURN TO THE QUIET
                            </button>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
}
