import React, { useState, useEffect, useMemo } from 'react';
import { Camera, Calendar, BookOpen, PlusCircle, Clock, Coffee, MapPin, ClipboardList, ChevronLeft, ChevronRight, X, Info } from 'lucide-react';

// 主程式組件
export default function App() {
  const [view, setView] = useState('add'); // 'add', 'history', 'guide'
  const [records, setRecords] = useState([]);
  const [selectedMonth, setSelectedMonth] = useState(new Date().getMonth());
  const [selectedYear, setSelectedYear] = useState(new Date().getFullYear());
  const [detailRecord, setDetailRecord] = useState(null);

  // 初始化今日日期
  const getTodayStr = () => new Date().toISOString().split('T')[0];

  // 表單狀態
  const [formData, setFormData] = useState({
    date: getTodayStr(),
    photo: null,
    brewTime: '',
    brewMethod: 'V60',
    roastLevel: '淺烘焙',
    origin: '',
    notes: ''
  });

  // 處理相片上傳
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

  // 儲存紀錄
  const handleSubmit = (e) => {
    e.preventDefault();
    const newRecord = {
      ...formData,
      id: Date.now(),
      timestamp: new Date(formData.date).getTime()
    };
    setRecords([newRecord, ...records]);
    // 重置表單
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

  // 過濾月份紀錄
  const filteredRecords = useMemo(() => {
    return records.filter(record => {
      const d = new Date(record.date);
      return d.getMonth() === selectedMonth && d.getFullYear() === selectedYear;
    });
  }, [records, selectedMonth, selectedYear]);

  // 切換月份
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
    <div className="min-h-screen font-sans text-slate-800 relative">
      {/* 背景層 - 海邊咖啡店 */}
      <div 
        className="fixed inset-0 z-0 bg-cover bg-center pointer-events-none"
        style={{ 
          backgroundImage: 'url("https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&q=80&w=2073")',
          opacity: 0.15
        }}
      />

      <div className="relative z-10 max-w-2xl mx-auto px-4 py-8">
        {/* Header */}
        <header className="text-center mb-8">
          <h1 className="text-4xl font-bold text-amber-900 mb-2 drop-shadow-sm">오늘커피 ☕️</h1>
          <p className="text-amber-700 tracking-widest uppercase text-sm">Today’s Coffee</p>
        </header>

        {/* Navigation */}
        <nav className="flex bg-white/60 backdrop-blur-md rounded-2xl p-1 mb-8 shadow-sm border border-white/50">
          <button 
            onClick={() => setView('add')}
            className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'add' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
          >
            <PlusCircle size={18} /> 紀錄
          </button>
          <button 
            onClick={() => setView('history')}
            className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'history' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
          >
            <Calendar size={18} /> 月曆
          </button>
          <button 
            onClick={() => setView('guide')}
            className={`flex-1 py-3 flex items-center justify-center gap-2 rounded-xl transition-all ${view === 'guide' ? 'bg-amber-600 text-white shadow-md' : 'text-slate-600 hover:bg-white/40'}`}
          >
            <BookOpen size={18} /> 指南
          </button>
        </nav>

        {/* 主要內容區 */}
        <main className="pb-20">
          
          {/* 1. 新增紀錄功能 */}
          {view === 'add' && (
            <div className="bg-white/80 backdrop-blur-md rounded-3xl p-6 shadow-xl border border-white/80">
              <h2 className="text-xl font-bold mb-6 flex items-center gap-2 border-b pb-4">
                <Coffee className="text-amber-600" /> 紀錄今天的味道
              </h2>
              
              <form onSubmit={handleSubmit} className="space-y-5">
                {/* 照片上傳 */}
                <div className="relative group">
                  <input 
                    type="file" 
                    accept="image/*" 
                    onChange={handlePhotoUpload}
                    className="absolute inset-0 w-full h-full opacity-0 cursor-pointer z-10"
                  />
                  <div className="w-full aspect-square md:aspect-video rounded-2xl border-2 border-dashed border-amber-200 bg-amber-50/30 flex flex-col items-center justify-center overflow-hidden transition-all group-hover:bg-amber-50">
                    {formData.photo ? (
                      <img src={formData.photo} alt="Preview" className="w-full h-full object-cover" />
                    ) : (
                      <>
                        <Camera size={40} className="text-amber-300 mb-2" />
                        <p className="text-amber-500 font-medium">點擊上傳咖啡美照</p>
                      </>
                    )}
                  </div>
                </div>

                {/* 日期選擇 */}
                <div className="flex flex-col gap-1">
                  <label className="text-sm font-semibold text-slate-500 flex items-center gap-1">
                    <Calendar size={14} /> 日期
                  </label>
                  <input 
                    type="date" 
                    value={formData.date}
                    onChange={(e) => setFormData({...formData, date: e.target.value})}
                    className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500"
                  />
                </div>

                <div className="grid grid-cols-2 gap-4">
                  {/* 沖煮方式 */}
                  <div className="flex flex-col gap-1">
                    <label className="text-sm font-semibold text-slate-500 flex items-center gap-1">
                      <ClipboardList size={14} /> 沖煮方式
                    </label>
                    <select 
                      value={formData.brewMethod}
                      onChange={(e) => setFormData({...formData, brewMethod: e.target.value})}
                      className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 appearance-none"
                    >
                      <option>V60</option>
                      <option>摺紙濾杯</option>
                      <option>法蘭絨</option>
                      <option>浸泡式</option>
                      <option>愛樂壓</option>
                    </select>
                  </div>

                  {/* 烘焙度 */}
                  <div className="flex flex-col gap-1">
                    <label className="text-sm font-semibold text-slate-500 flex items-center gap-1">
                      <Coffee size={14} /> 烘焙度
                    </label>
                    <select 
                      value={formData.roastLevel}
                      onChange={(e) => setFormData({...formData, roastLevel: e.target.value})}
                      className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 appearance-none"
                    >
                      <option>極淺烘焙</option>
                      <option>淺烘焙</option>
                      <option>中烘焙</option>
                      <option>深烘焙</option>
                    </select>
                  </div>
                </div>

                <div className="grid grid-cols-2 gap-4">
                  {/* 沖煮時間 */}
                  <div className="flex flex-col gap-1">
                    <label className="text-sm font-semibold text-slate-500 flex items-center gap-1">
                      <Clock size={14} /> 沖煮時間
                    </label>
                    <input 
                      type="text" 
                      placeholder="例如: 2:30"
                      value={formData.brewTime}
                      onChange={(e) => setFormData({...formData, brewTime: e.target.value})}
                      className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500"
                    />
                  </div>

                  {/* 產區或名稱 */}
                  <div className="flex flex-col gap-1">
                    <label className="text-sm font-semibold text-slate-500 flex items-center gap-1">
                      <MapPin size={14} /> 產區 / 名稱
                    </label>
                    <input 
                      type="text" 
                      placeholder="例如: 衣索比亞"
                      value={formData.origin}
                      onChange={(e) => setFormData({...formData, origin: e.target.value})}
                      className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500"
                    />
                  </div>
                </div>

                {/* 心得 */}
                <div className="flex flex-col gap-1">
                  <label className="text-sm font-semibold text-slate-500">心得與筆記</label>
                  <textarea 
                    rows="3"
                    placeholder="今天的咖啡喝起來..."
                    value={formData.notes}
                    onChange={(e) => setFormData({...formData, notes: e.target.value})}
                    className="w-full p-3 rounded-xl bg-slate-50 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 resize-none"
                  ></textarea>
                </div>

                <button 
                  type="submit"
                  className="w-full bg-amber-700 text-white py-4 rounded-2xl font-bold shadow-lg shadow-amber-900/20 hover:bg-amber-800 transition-colors mt-4"
                >
                  儲存紀錄
                </button>
              </form>
            </div>
          )}

          {/* 2. 月份歷史紀錄 */}
          {view === 'history' && (
            <div className="space-y-6">
              {/* 月份導航 */}
              <div className="flex items-center justify-between bg-white/80 backdrop-blur-md p-4 rounded-2xl shadow-sm border border-white/80">
                <button onClick={() => changeMonth(-1)} className="p-2 hover:bg-amber-100 rounded-full transition-colors">
                  <ChevronLeft />
                </button>
                <h2 className="text-lg font-bold text-amber-900">
                  {selectedYear} 年 {selectedMonth + 1} 月
                </h2>
                <button onClick={() => changeMonth(1)} className="p-2 hover:bg-amber-100 rounded-full transition-colors">
                  <ChevronRight />
                </button>
              </div>

              {/* 紀錄網格 */}
              <div className="grid grid-cols-2 md:grid-cols-3 gap-4">
                {filteredRecords.length > 0 ? (
                  filteredRecords.map(record => (
                    <div 
                      key={record.id} 
                      onClick={() => setDetailRecord(record)}
                      className="group cursor-pointer bg-white/80 backdrop-blur-sm rounded-2xl overflow-hidden shadow-sm border border-white/80 hover:shadow-md transition-all"
                    >
                      <div className="aspect-square bg-slate-100 overflow-hidden relative">
                        {record.photo ? (
                          <img src={record.photo} alt="Brew" className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" />
                        ) : (
                          <div className="w-full h-full flex items-center justify-center text-slate-300">
                            <Coffee size={32} />
                          </div>
                        )}
                        <div className="absolute top-2 left-2 bg-black/40 text-white text-[10px] px-2 py-1 rounded-full backdrop-blur-sm">
                          {record.date.split('-')[2]}日
                        </div>
                      </div>
                      <div className="p-3">
                        <p className="text-xs font-bold truncate text-slate-800">{record.origin || '無標題'}</p>
                        <p className="text-[10px] text-slate-500 truncate">{record.brewMethod}</p>
                      </div>
                    </div>
                  ))
                ) : (
                  <div className="col-span-full py-20 text-center bg-white/40 rounded-3xl border border-dashed border-slate-300">
                    <p className="text-slate-500 italic">這個月還沒有紀錄喔～</p>
                    <button onClick={() => setView('add')} className="mt-4 text-amber-700 font-bold flex items-center gap-1 mx-auto">
                      <PlusCircle size={16} /> 去寫第一篇
                    </button>
                  </div>
                )}
              </div>
            </div>
          )}

          {/* 3. 沖煮指南 */}
          {view === 'guide' && (
            <div className="space-y-6">
              <div className="bg-white/90 backdrop-blur-md rounded-3xl p-8 shadow-xl border border-white/80">
                <h2 className="text-2xl font-bold text-amber-900 mb-6 flex items-center gap-2">
                  <Info className="text-amber-600" /> 手沖咖啡指南
                </h2>

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
                    <h3 className="text-lg font-bold text-amber-800 mb-4 flex items-center gap-2 border-l-4 border-amber-600 pl-3">沖煮步驟 (4:6 簡易法)</h3>
                    <div className="space-y-4">
                      {[
                        { step: "01", title: "悶蒸", content: "注入約 2 倍粉量的水（40ml），等待 30 秒。" },
                        { step: "02", title: "第一段萃取", content: "穩定繞圈注水至 120ml。此段影響風味酸甜感。" },
                        { step: "03", title: "第二段萃取", content: "水流乾後，注入至 200ml。維持溫度與流速。" },
                        { step: "04", title: "最後注水", content: "分次注入剩餘水分至 300ml，總時約 2:30-3:00。" }
                      ].map((item, idx) => (
                        <div key={idx} className="flex gap-4">
                          <span className="text-2xl font-black text-amber-200 italic leading-none">{item.step}</span>
                          <div>
                            <p className="font-bold text-slate-800 mb-1">{item.title}</p>
                            <p className="text-sm text-slate-600 leading-relaxed">{item.content}</p>
                          </div>
                        </div>
                      ))}
                    </div>
                  </section>
                </div>
              </div>
            </div>
          )}
        </main>

        {/* 紀錄詳情 Modal */}
        {detailRecord && (
          <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
            <div className="absolute inset-0 bg-black/60 backdrop-blur-sm" onClick={() => setDetailRecord(null)}></div>
            <div className="relative w-full max-w-lg bg-white rounded-3xl overflow-hidden shadow-2xl animate-in fade-in zoom-in duration-300">
              <button 
                onClick={() => setDetailRecord(null)}
                className="absolute top-4 right-4 z-10 p-2 bg-black/20 text-white rounded-full hover:bg-black/40 transition-colors"
              >
                <X size={20} />
              </button>

              <div className="h-64 bg-slate-200 relative">
                {detailRecord.photo ? (
                  <img src={detailRecord.photo} alt="Detail" className="w-full h-full object-cover" />
                ) : (
                  <div className="w-full h-full flex items-center justify-center text-slate-400">
                    <Coffee size={64} />
                  </div>
                )}
              </div>

              <div className="p-6">
                <div className="flex justify-between items-start mb-6">
                  <div>
                    <p className="text-xs text-amber-600 font-bold uppercase tracking-wider mb-1">{detailRecord.date}</p>
                    <h3 className="text-2xl font-black text-slate-900">{detailRecord.origin || "無名咖啡豆"}</h3>
                  </div>
                  <div className="bg-amber-100 text-amber-800 px-3 py-1 rounded-full text-xs font-bold">
                    {detailRecord.roastLevel}
                  </div>
                </div>

                <div className="grid grid-cols-2 gap-6 mb-6">
                  <div className="flex items-center gap-3">
                    <div className="p-2 bg-slate-100 rounded-lg text-slate-500">
                      <Clock size={20} />
                    </div>
                    <div>
                      <p className="text-[10px] text-slate-400 font-bold uppercase">沖煮時間</p>
                      <p className="text-sm font-bold text-slate-700">{detailRecord.brewTime || '--'}</p>
                    </div>
                  </div>
                  <div className="flex items-center gap-3">
                    <div className="p-2 bg-slate-100 rounded-lg text-slate-500">
                      <ClipboardList size={20} />
                    </div>
                    <div>
                      <p className="text-[10px] text-slate-400 font-bold uppercase">沖煮方式</p>
                      <p className="text-sm font-bold text-slate-700">{detailRecord.brewMethod}</p>
                    </div>
                  </div>
                </div>

                <div className="bg-slate-50 p-4 rounded-2xl border border-slate-100">
                  <p className="text-xs text-slate-400 font-bold mb-2 uppercase tracking-wide">品飲心得</p>
                  <p className="text-slate-700 leading-relaxed italic">
                    {detailRecord.notes || "今天這杯咖啡很安靜，沒有留下什麼話。"}
                  </p>
                </div>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
