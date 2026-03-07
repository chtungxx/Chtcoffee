<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>오늘커피 ☕️ Today’s Coffee</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- React & ReactDOM -->
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <!-- Babel for JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700;900&display=swap');
        body {
            font-family: 'Noto Sans TC', sans-serif;
        }
        .hide-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .animate-in {
            animation: fadeIn 0.3s ease-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useMemo } = React;

        // 手動定義需要使用的 Lucide 圖示組件 (因為 CDN 環境下 React 版 Lucide 載入較複雜)
        const Icon = ({ name, size = 24, className = "" }) => {
            useEffect(() => {
                if (window.lucide) {
                    window.lucide.createIcons();
                }
            }, [name]);
            return <i data-lucide={name} className={className} style={{ width: size, height: size }}></i>;
        };

        function App() {
            const [view, setView] = useState('add');
            const [records, setRecords] = useState([]);
            const [selectedMonth, setSelectedMonth] = useState(new Date().getMonth());
            const [selectedYear, setSelectedYear] = useState(new Date().getFullYear());
            const [detailRecord, setDetailRecord] = useState(null);

            // 從 localStorage 讀取紀錄
            useEffect(() => {
                const saved = localStorage.getItem('coffee_records');
                if (saved) {
                    try {
                        setRecords(JSON.parse(saved));
                    } catch (e) {
                        console.error("Failed to load records", e);
                    }
                }
            }, []);

            // 儲存紀錄至 localStorage
            useEffect(() => {
                localStorage.setItem('coffee_records', JSON.stringify(records));
            }, [records]);

            const getTodayStr = () => new Date().toISOString().split('T')[0];

            const [formData, setFormData] = useState({
                date: getTodayStr(),
                photo: null,
                brewTime: '',
                brewMethod: 'V60',
                roastLevel: '淺烘焙',
                origin: '',
                notes: ''
            });

            const handlePhotoUpload = (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onloadend = () => {
                        setFormData({ ...formData, photo: reader.result });
                    };
                    reader.readAsDataURL(file);
                }
            };

            const handleSubmit = (e) => {
                e.preventDefault();
                const newRecord = {
                    ...formData,
                    id: Date.now(),
                    timestamp: new Date(formData.date).getTime()
                };
                setRecords([newRecord, ...records]);
                setFormData({
                    date: getTodayStr(),
                    photo: null,
                    brewTime: '',
                    brewMethod: 'V60',
                    roastLevel: '淺烘焙',
                    origin: '',
                    notes: ''
                });
                setView('history');
            };

            const filteredRecords = useMemo(() => {
                return records.filter(record => {
                    const d = new Date(record.date);
                    return d.getMonth() === selectedMonth && d.getFullYear() === selectedYear;
                });
            }, [records, selectedMonth, selectedYear]);

            const changeMonth = (offset) => {
                let newMonth = selectedMonth + offset;
                let newYear = selectedYear;
                if (newMonth < 0) {
                    newMonth = 11;
                    newYear -= 1;
                } else if (newMonth > 11) {
                    newMonth = 0;
                    newYear += 1;
                }
                setSelectedMonth(newMonth);
                setSelectedYear(newYear);
            };

            return (
                <div className="min-h-screen text-slate-800 relative">
                    {/* 背景層 */}
                    <div 
                        className="fixed inset-0 z-0 bg-cover bg-center pointer-events-none"
                        style={{ 
                            backgroundImage: 'url("https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&q=80&w=2073")',
                            opacity: 0.15
                        }}
                    />

                    <div className="relative z-10 max-w-2xl mx-auto px-4 py-8">
                        <header className="text-center mb-8">
                            <h1 className="text-4xl font-bold text-amber-900 mb-2 drop-shadow-sm">오늘커피 ☕️</h1>
                            <p className="text-amber-700 tracking-widest uppercase text-sm">Today’s Coffee</p>
                        </header>

                        <nav className="flex bg-white/60 backdrop-blur-md rounded-2xl p-1 mb-8 shadow-sm border border-white/50">
                            <button 
                                onClick={() => setView('add')}
                                className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'add' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
                            >
                                <Icon name="plus-circle" size={18} /> 紀錄
                            </button>
                            <button 
                                onClick={() => setView('history')}
                                className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'history' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
                            >
                                <Icon name="calendar" size={18} /> 月曆
                            </button>
                            <button 
                                onClick={() => setView('guide')}
                                className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'guide' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
                            >
                                <Icon name="book-open" size={18} /> 指南
                            </button>
                        </nav>

                        <main className="pb-20">
                            {view === 'add' && (
                                <div className="bg-white/80 backdrop-blur-md rounded-3xl p-6 shadow-xl border border-white/80 animate-in">
                                    <h2 className="text-xl font-bold mb-6 flex items-center gap-2 border-b pb-4">
                                        <Icon name="coffee" className="text-amber-600" /> 紀錄今天的味道
                                    </h2>
                                    
                                    <form onSubmit={handleSubmit} className="space-y-5">
                                        <div className="relative group">
                                            <input type="file" accept="image/*" onChange={handlePhotoUpload} className="absolute inset-0 w-full h-full opacity-0 cursor-pointer z-10" />
                                            <div className="w-full aspect-square md:aspect-video rounded-2xl border-2 border-dashed border-amber-200 bg-amber-50/30 flex flex-col items-center justify-center overflow-hidden transition-all group-hover:bg-amber-50">
                                                {formData.photo ? (
                                                    <img src={formData.photo} alt="Preview" className="w-full h-full object-cover" />
                                                ) : (
                                                    <>
                                                        <Icon name="camera" size={40} className="text-amber-300 mb-2" />
                                                        <p className="text-amber-500 font-medium text-sm">點擊上傳咖啡美照</p>
                                                    </>
                                                )}
                                            </div>
                                        </div>

                                        <div className="flex flex-col gap-1">
                                            <label className="text-xs font-bold text-slate-500 flex items-center gap-1 uppercase tracking-wider"><Icon name="calendar" size={14} /> 日期</label>
                                            <input type="date" value={formData.date} onChange={(e) => setFormData({...formData, date: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500" />
                                        </div>

                                        <div className="grid grid-cols-2 gap-4">
                                            <div className="flex flex-col gap-1">
                                                <label className="text-xs font-bold text-slate-500 flex items-center gap-1 uppercase tracking-wider"><Icon name="clipboard-list" size={14} /> 沖煮方式</label>
                                                <select value={formData.brewMethod} onChange={(e) => setFormData({...formData, brewMethod: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 appearance-none">
                                                    <option>V60</option><option>摺紙濾杯</option><option>法蘭絨</option><option>浸泡式</option><option>愛樂壓</option>
                                                </select>
                                            </div>
                                            <div className="flex flex-col gap-1">
                                                <label className="text-xs font-bold text-slate-500 flex items-center gap-1 uppercase tracking-wider"><Icon name="coffee" size={14} /> 烘焙度</label>
                                                <select value={formData.roastLevel} onChange={(e) => setFormData({...formData, roastLevel: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 appearance-none">
                                                    <option>極淺烘焙</option><option>淺烘焙</option><option>中烘焙</option><option>深烘焙</option>
                                                </select>
                                            </div>
                                        </div>

                                        <div className="grid grid-cols-2 gap-4">
                                            <div className="flex flex-col gap-1">
                                                <label className="text-xs font-bold text-slate-500 flex items-center gap-1 uppercase tracking-wider"><Icon name="clock" size={14} /> 沖煮時間</label>
                                                <input type="text" placeholder="2:30" value={formData.brewTime} onChange={(e) => setFormData({...formData, brewTime: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500" />
                                            </div>
                                            <div className="flex flex-col gap-1">
                                                <label className="text-xs font-bold text-slate-500 flex items-center gap-1 uppercase tracking-wider"><Icon name="map-pin" size={14} /> 產區 / 名稱</label>
                                                <input type="text" placeholder="衣索比亞" value={formData.origin} onChange={(e) => setFormData({...formData, origin: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500" />
                                            </div>
                                        </div>

                                        <div className="flex flex-col gap-1">
                                            <label className="text-xs font-bold text-slate-500 uppercase tracking-wider">心得與筆記</label>
                                            <textarea rows="3" placeholder="今天的咖啡喝起來..." value={formData.notes} onChange={(e) => setFormData({...formData, notes: e.target.value})} className="w-full p-3 rounded-xl bg-slate-50/50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 resize-none"></textarea>
                                        </div>

                                        <button type="submit" className="w-full bg-amber-700 text-white py-4 rounded-2xl font-bold shadow-lg shadow-amber-900/20 hover:bg-amber-800 transition-colors mt-4">儲存紀錄</button>
                                    </form>
                                </div>
                            )}

                            {view === 'history' && (
                                <div className="space-y-6 animate-in">
                                    <div className="flex items-center justify-between bg-white/80 backdrop-blur-md p-4 rounded-2xl shadow-sm border border-white/80">
                                        <button onClick={() => changeMonth(-1)} className="p-2 hover:bg-amber-100 rounded-full transition-colors"><Icon name="chevron-left" /></button>
                                        <h2 className="text-lg font-bold text-amber-900">{selectedYear} 年 {selectedMonth + 1} 月</h2>
                                        <button onClick={() => changeMonth(1)} className="p-2 hover:bg-amber-100 rounded-full transition-colors"><Icon name="chevron-right" /></button>
                                    </div>

                                    <div className="grid grid-cols-2 md:grid-cols-3 gap-4">
                                        {filteredRecords.length > 0 ? (
                                            filteredRecords.map(record => (
                                                <div key={record.id} onClick={() => setDetailRecord(record)} className="group cursor-pointer bg-white/80 backdrop-blur-sm rounded-2xl overflow-hidden shadow-sm border border-white/80 hover:shadow-md transition-all">
                                                    <div className="aspect-square bg-slate-100 overflow-hidden relative">
                                                        {record.photo ? (
                                                            <img src={record.photo} className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" />
                                                        ) : (
                                                            <div className="w-full h-full flex items-center justify-center text-slate-300"><Icon name="coffee" size={32} /></div>
                                                        )}
                                                        <div className="absolute top-2 left-2 bg-black/40 text-white text-[10px] px-2 py-1 rounded-full backdrop-blur-sm">{record.date.split('-')[2]}日</div>
                                                    </div>
                                                    <div className="p-3">
                                                        <p className="text-xs font-bold truncate text-slate-800">{record.origin || '今日咖啡'}</p>
                                                        <p className="text-[10px] text-slate-500 truncate">{record.brewMethod}</p>
                                                    </div>
                                                </div>
                                            ))
                                        ) : (
                                            <div className="col-span-full py-20 text-center bg-white/40 rounded-3xl border border-dashed border-slate-300">
                                                <p className="text-slate-500 italic">這個月還沒有紀錄喔～</p>
                                                <button onClick={() => setView('add')} className="mt-4 text-amber-700 font-bold flex items-center justify-center gap-1 mx-auto text-sm"><Icon name="plus-circle" size={16} /> 去寫第一篇</button>
                                            </div>
                                        )}
                                    </div>
                                </div>
                            )}

                            {view === 'guide' && (
                                <div className="bg-white/90 backdrop-blur-md rounded-3xl p-8 shadow-xl border border-white/80 animate-in">
                                    <h2 className="text-2xl font-bold text-amber-900 mb-6 flex items-center gap-2"><Icon name="info" className="text-amber-600" /> 手沖咖啡指南</h2>
                                    <div className="space-y-8">
                                        <section>
                                            <h3 className="text-lg font-bold text-amber-800 mb-3 flex items-center gap-2 border-l-4 border-amber-600 pl-3">黃金比例</h3>
                                            <div className="grid grid-cols-2 gap-4">
                                                <div className="bg-amber-50 p-4 rounded-2xl border border-amber-100">
                                                    <p className="text-xs text-amber-600 font-bold mb-1">粉水比</p>
                                                    <p className="text-xl font-black text-amber-900">1 : 15</p>
                                                    <p className="text-[10px] text-amber-700 mt-1">例如：20g 粉對 300ml 水</p>
                                                </div>
                                                <div className="bg-amber-50 p-4 rounded-2xl border border-amber-100">
                                                    <p className="text-xs text-amber-600 font-bold mb-1">建議水溫</p>
                                                    <p className="text-xl font-black text-amber-900">90 - 92°C</p>
                                                    <p className="text-[10px] text-amber-700 mt-1">淺烘焙建議較高溫</p>
                                                </div>
                                            </div>
                                        </section>
                                        <section>
                                            <h3 className="text-lg font-bold text-amber-800 mb-4 flex items-center gap-2 border-l-4 border-amber-600 pl-3">沖煮步驟 (4:6 法)</h3>
                                            <div className="space-y-4">
                                                {[
                                                    { s: "01", t: "悶蒸", c: "注入約 2 倍粉量的水（40ml），等待 30 秒。" },
                                                    { s: "02", t: "第一段萃取", c: "穩定繞圈注水至 120ml。此段影響風味酸甜感。" },
                                                    { s: "03", t: "第二段萃取", c: "水流乾後，注入至 200ml。維持溫度與流速。" },
                                                    { s: "04", t: "最後注水", c: "分次注入剩餘水分至 300ml，總時約 2:30-3:00。" }
                                                ].map((item, idx) => (
                                                    <div key={idx} className="flex gap-4">
                                                        <span className="text-2xl font-black text-amber-200 italic leading-none">{item.s}</span>
                                                        <div><p className="font-bold text-slate-800 mb-1 text-sm">{item.t}</p><p className="text-xs text-slate-600 leading-relaxed">{item.c}</p></div>
                                                    </div>
                                                ))}
                                            </div>
                                        </section>
                                    </div>
                                </div>
                            )}
                        </main>

                        {detailRecord && (
                            <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
                                <div className="absolute inset-0 bg-black/60 backdrop-blur-sm" onClick={() => setDetailRecord(null)}></div>
                                <div className="relative w-full max-w-lg bg-white rounded-3xl overflow-hidden shadow-2xl animate-in">
                                    <button onClick={() => setDetailRecord(null)} className="absolute top-4 right-4 z-10 p-2 bg-black/20 text-white rounded-full"><Icon name="x" size={20} /></button>
                                    <div className="h-64 bg-slate-200 relative">
                                        {detailRecord.photo ? <img src={detailRecord.photo} className="w-full h-full object-cover" /> : <div className="w-full h-full flex items-center justify-center text-slate-400"><Icon name="coffee" size={64} /></div>}
                                    </div>
                                    <div className="p-6">
                                        <div className="flex justify-between items-start mb-6">
                                            <div><p className="text-[10px] text-amber-600 font-bold uppercase tracking-wider mb-1">{detailRecord.date}</p><h3 className="text-2xl font-black text-slate-900">{detailRecord.origin || "今日咖啡"}</h3></div>
                                            <div className="bg-amber-100 text-amber-800 px-3 py-1 rounded-full text-[10px] font-bold">{detailRecord.roastLevel}</div>
                                        </div>
                                        <div className="grid grid-cols-2 gap-6 mb-6">
                                            <div className="flex items-center gap-3">
                                                <div className="p-2 bg-slate-100 rounded-lg text-slate-500"><Icon name="clock" size={20} /></div>
                                                <div><p className="text-[10px] text-slate-400 font-bold uppercase">時間</p><p className="text-sm font-bold text-slate-700">{detailRecord.brewTime || '--'}</p></div>
                                            </div>
                                            <div className="flex items-center gap-3">
                                                <div className="p-2 bg-slate-100 rounded-lg text-slate-500"><Icon name="clipboard-list" size={20} /></div>
                                                <div><p className="text-[10px] text-slate-400 font-bold uppercase">方式</p><p className="text-sm font-bold text-slate-700">{detailRecord.brewMethod}</p></div>
                                            </div>
                                        </div>
                                        <div className="bg-slate-50 p-4 rounded-2xl border border-slate-100">
                                            <p className="text-xs text-slate-400 font-bold mb-2 uppercase tracking-wide">筆記</p>
                                            <p className="text-sm text-slate-700 leading-relaxed italic">{detailRecord.notes || "這杯咖啡沒有留下話語。"}</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        )}
                    </div>
                </div>
            );
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
