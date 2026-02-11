import React, { useState } from 'react';
import { Sparkles, ArrowRight, Github, Twitter, Eye, EyeOff, Mail } from 'lucide-react';

interface AuthScreenProps {
  onLogin: () => void;
}

const AuthScreen: React.FC<AuthScreenProps> = ({ onLogin }) => {
  const [isRegistering, setIsRegistering] = useState(false);
  const [showPassword, setShowPassword] = useState(false);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
    <div className="min-h-screen w-full bg-[#FAF9F6] flex items-center justify-center p-4 relative overflow-hidden">
      {/* Background Blobs */}
      <div className="absolute top-[-10%] left-[-5%] w-[40%] h-[40%] bg-[#F3E5F5] rounded-full blur-[120px] opacity-60 animate-pulse"></div>
      <div className="absolute bottom-[-10%] right-[-5%] w-[40%] h-[40%] bg-[#E0F2F1] rounded-full blur-[120px] opacity-60 animate-pulse" style={{ animationDelay: '2s' }}></div>

      <div className="w-full max-w-lg bg-white/40 backdrop-blur-3xl border border-white/50 rounded-[3rem] p-10 md:p-14 shadow-[0_32px_64px_-16px_rgba(0,0,0,0.05)] relative z-10 transition-all">
        <div className="flex flex-col items-center mb-10">
          <div className="w-16 h-16 bg-gradient-to-br from-[#F3E5F5] to-[#E0F2F1] rounded-2xl flex items-center justify-center shadow-inner mb-6">
            <Sparkles size={32} className="text-[#7B1FA2]" />
          </div>
          <h1 className="text-4xl font-bold tracking-tight text-[#2D3436] text-center">
            {isRegistering ? 'Start your journey' : 'Welcome to ZenNotes'}
          </h1>
          <p className="text-[#636E72] mt-3 font-medium text-center">Your mind's quiet corner for bright ideas.</p>
        </div>

        <form className="space-y-6" onSubmit={(e) => { e.preventDefault(); onLogin(); }}>
          <div className="space-y-4">
            <div className="space-y-1.5">
              <label className="text-xs font-black text-gray-400 uppercase tracking-widest ml-1 flex items-center gap-2">
                <Mail size={12} /> Email Address
              </label>
              <input 
                type="email" 
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                placeholder="Enter your email" 
                className="w-full bg-white/60 border border-white rounded-2xl py-4 px-6 text-sm font-medium text-[#2D3436] placeholder:text-gray-300 focus:bg-white focus:ring-4 focus:ring-[#F3E5F5] transition-all outline-none"
                required
              />
            </div>

            <div className="space-y-1.5 relative">
              <label className="text-xs font-black text-gray-400 uppercase tracking-widest ml-1">Password</label>
              <div className="relative group">
                <input 
                  type={showPassword ? "text" : "password"} 
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  placeholder="Your password" 
                  className="w-full bg-white/60 border border-white rounded-2xl py-4 px-6 pr-14 text-sm font-medium text-[#2D3436] placeholder:text-gray-300 focus:bg-white focus:ring-4 focus:ring-[#F3E5F5] transition-all outline-none"
                  required
                />
                <button
                  type="button"
                  onClick={() => setShowPassword(!showPassword)}
                  className="absolute right-4 top-1/2 -translate-y-1/2 p-2 text-gray-400 hover:text-[#7B1FA2] transition-colors rounded-xl"
                  title={showPassword ? "Hide password" : "Show password"}
                >
                  {showPassword ? <EyeOff size={20} /> : <Eye size={20} />}
                </button>
              </div>
            </div>
          </div>

          <button 
            type="submit"
            className="w-full py-4 bg-[#2D3436] text-white rounded-2xl font-bold flex items-center justify-center gap-3 hover:bg-[#1e2324] hover:shadow-xl hover:translate-y-[-2px] transition-all"
          >
            {isRegistering ? 'Create Account' : 'Sign In'}
            <ArrowRight size={20} />
          </button>
        </form>

        <div className="mt-8 flex items-center gap-4">
          <div className="h-[1px] flex-1 bg-gray-100"></div>
          <span className="text-xs font-bold text-gray-400 uppercase tracking-widest">Social Connection</span>
          <div className="h-[1px] flex-1 bg-gray-100"></div>
        </div>

        <div className="mt-8 flex justify-center gap-4">
          <button className="flex-1 py-3 px-6 bg-white border border-gray-100 rounded-2xl flex items-center justify-center gap-2 hover:bg-gray-50 transition-colors">
            <Github size={18} className="text-gray-700" />
          </button>
          <button className="flex-1 py-3 px-6 bg-white border border-gray-100 rounded-2xl flex items-center justify-center gap-2 hover:bg-gray-50 transition-colors">
            <Twitter size={18} className="text-[#1DA1F2]" />
          </button>
        </div>

        <p className="text-center mt-10 text-sm text-gray-500 font-medium">
          {isRegistering ? 'Already have an account?' : 'New to ZenNotes?'}
          <button 
            onClick={() => setIsRegistering(!isRegistering)}
            className="ml-2 text-[#7B1FA2] hover:underline font-bold"
          >
            {isRegistering ? 'Sign In' : 'Join for free'}
          </button>
        </p>
      </div>
    </div>
  );
};

export default AuthScreen;

import React from 'react';
import { Note, Space } from '../types';
import { Search, Plus, Trash2, Clock, ChevronRight, Hash } from 'lucide-react';

interface DashboardProps {
  notes: Note[];
  spaces: Space[];
  activeSpaceId: string | null;
  onNoteClick: (id: string) => void;
  onDeleteNote: (id: string) => void;
  onCreateNote: () => void;
}

const Dashboard: React.FC<DashboardProps> = ({ notes, spaces, activeSpaceId, onNoteClick, onDeleteNote, onCreateNote }) => {
  const currentSpace = spaces.find(s => s.id === activeSpaceId);

  return (
    <div className="max-w-7xl mx-auto animate-in fade-in slide-in-from-bottom-4 duration-700">
      <div className="flex flex-col md:flex-row md:items-end justify-between gap-6 mb-12">
        <div className="space-y-4">
          <div className="inline-flex items-center gap-2 bg-[#F3E5F5] px-4 py-1.5 rounded-full text-[#7B1FA2] text-xs font-black uppercase tracking-widest border border-white">
            <span className="w-1.5 h-1.5 rounded-full bg-[#7B1FA2] animate-pulse"></span>
            {currentSpace ? currentSpace.name : 'All Notes'}
          </div>
          <h1 className="text-5xl font-black text-[#2D3436] tracking-tight">
            {currentSpace ? `Focus on ${currentSpace.name}` : 'Welcome back, dreamer.'}
          </h1>
          <p className="text-gray-400 text-lg font-medium">Capture your sparks before they fade away.</p>
        </div>
      </div>

      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
        {/* Empty State / Create */}
        <button 
          onClick={onCreateNote}
          className="group h-[280px] bg-white border-2 border-dashed border-gray-100 rounded-[2.5rem] flex flex-col items-center justify-center gap-4 hover:border-[#F3E5F5] hover:bg-[#FDFCFB] transition-all"
        >
          <div className="w-16 h-16 bg-[#F3E5F5] text-[#7B1FA2] rounded-3xl flex items-center justify-center group-hover:scale-110 transition-transform">
            <Plus size={32} strokeWidth={2.5} />
          </div>
          <p className="text-sm font-black text-gray-400 group-hover:text-[#7B1FA2]">New Thought</p>
        </button>

        {notes.map((note, i) => {
          const space = spaces.find(s => s.id === note.spaceId);
          const pastelBgs = ['bg-[#F3E5F5]', 'bg-[#E0F2F1]', 'bg-[#FFF3E0]', 'bg-[#E3F2FD]', 'bg-[#F1F8E9]'];
          const bgColor = pastelBgs[i % pastelBgs.length];

          return (
            <div 
              key={note.id}
              onClick={() => onNoteClick(note.id)}
              className={`group relative ${bgColor} rounded-[2.5rem] p-8 min-h-[280px] flex flex-col justify-between hover:shadow-2xl hover:shadow-[#F3E5F5]/40 hover:-translate-y-2 transition-all cursor-pointer animate-in fade-in zoom-in-95 duration-500`}
              style={{ animationDelay: `${i * 100}ms` }}
            >
              <div className="space-y-4">
                <div className="flex items-center justify-between">
                  <span className="px-3 py-1 bg-white/60 backdrop-blur-md rounded-xl text-[10px] font-black text-gray-600 uppercase tracking-widest">
                    {space?.name || 'Idea'}
                  </span>
                  <button 
                    onClick={(e) => { e.stopPropagation(); onDeleteNote(note.id); }}
                    className="opacity-0 group-hover:opacity-100 p-2 hover:bg-white/40 rounded-xl text-gray-600 transition-all"
                  >
                    <Trash2 size={16} />
                  </button>
                </div>
                <h3 className="text-2xl font-black text-[#2D3436] line-clamp-2 leading-tight">
                  {note.title || 'Untitled Entry'}
                </h3>
                <p className="text-gray-600/70 text-sm font-medium line-clamp-3 leading-relaxed">
                  {note.content || 'Tap to add detail to your spark...'}
                </p>
              </div>

              <div className="pt-6 border-t border-white/20 flex items-center justify-between">
                <div className="flex items-center gap-2 text-[10px] font-bold text-gray-500/60 uppercase">
                  <Clock size={12} />
                  <span>{new Date(note.lastModified).toLocaleDateString()}</span>
                </div>
                <div className="w-10 h-10 bg-white/40 rounded-full flex items-center justify-center text-gray-600">
                  <ChevronRight size={18} />
                </div>
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
};

export default Dashboard;


import React from 'react';
import { Search, Bell, Command, Sparkles } from 'lucide-react';
import { Note, UserProfile } from '../types';

interface HeaderProps {
  activeNote?: Note;
  profile: UserProfile;
  searchQuery: string;
  onSearchChange: (val: string) => void;
  onGoHome: () => void;
}

const Header: React.FC<HeaderProps> = ({ searchQuery, onSearchChange, profile, onGoHome }) => {
  return (
    <header className="h-24 px-12 flex items-center justify-between shrink-0 bg-[#FAF9F6]">
      <div className="flex items-center gap-8 flex-1">
        {/* Branding moved to top */}
        <div 
          onClick={onGoHome}
          className="flex items-center gap-3.5 group cursor-pointer shrink-0"
        >
          <div className="w-11 h-11 bg-gradient-to-br from-[#F3E5F5] to-[#E0F2F1] rounded-2xl flex items-center justify-center shadow-lg shadow-purple-100 group-hover:scale-105 transition-all">
            <Sparkles size={22} className="text-[#7B1FA2]" />
          </div>
          <span className="hidden sm:block text-2xl font-black text-[#2D3436] tracking-tight">ZenNotes</span>
        </div>

        <div className="flex-1 max-w-xl">
          <div className="relative group">
            <div className="absolute left-5 top-1/2 -translate-y-1/2 pointer-events-none">
              <Search className="text-gray-300 group-focus-within:text-[#7B1FA2] transition-colors" size={20} strokeWidth={2.5} />
            </div>
            <input 
              type="text" 
              placeholder="Find anything..."
              value={searchQuery}
              onChange={(e) => onSearchChange(e.target.value)}
              className="w-full bg-white border border-gray-100 rounded-[1.5rem] py-4 pl-14 pr-14 text-sm font-bold focus:ring-4 focus:ring-[#F3E5F5]/30 focus:border-[#F3E5F5] transition-all outline-none placeholder:text-gray-300 shadow-sm"
            />
            <div className="absolute right-5 top-1/2 -translate-y-1/2 hidden md:flex items-center gap-1.5 px-2 py-1 bg-gray-50 rounded-lg border border-gray-100">
              <Command size={10} className="text-gray-400" />
              <span className="text-[10px] font-black text-gray-400 tracking-tighter">F</span>
            </div>
          </div>
        </div>
      </div>

      <div className="flex items-center gap-8 ml-8">
        <button className="relative p-3 hover:bg-white rounded-2xl text-gray-300 hover:text-gray-500 transition-all group">
          <Bell size={22} />
          <span className="absolute top-3 right-3 w-2 h-2 bg-rose-400 rounded-full border-2 border-[#FAF9F6]"></span>
        </button>
        
        <div className="flex items-center gap-4 pl-8 border-l border-gray-100">
          <div className="text-right hidden sm:block">
            <p className="text-sm font-black text-[#2D3436]">{profile.name}</p>
            <p className="text-[10px] text-[#7B1FA2] font-black uppercase tracking-widest">Beginner</p>
          </div>
          <div className="w-12 h-12 rounded-[1.2rem] bg-gradient-to-tr from-[#F3E5F5] to-[#E0F2F1] p-0.5 shadow-lg shadow-gray-200 overflow-hidden">
            <div className="w-full h-full rounded-[1.1rem] overflow-hidden border-2 border-white">
              <img src={profile.avatar} alt="Avatar" className="w-full h-full object-cover" />
            </div>
          </div>
        </div>
      </div>
    </header>
  );
};

export default Header;


import React, { useState, useRef } from 'react';
import { Note } from '../types';
import { 
  ChevronLeft, 
  Sparkles, 
  Share2, 
  Save, 
  Bold,
  Italic,
  List,
  Highlighter,
  Maximize2,
  Wand2,
  CheckCircle2,
  Image as ImageIcon,
  X
} from 'lucide-react';
import { GoogleGenAI } from "@google/genai";

interface NoteEditorProps {
  note?: Note;
  onUpdateNote: (id: string, updates: Partial<Note>) => void;
  onBack: () => void;
}

const NoteEditor: React.FC<NoteEditorProps> = ({ note, onUpdateNote, onBack }) => {
  const [isAiLoading, setIsAiLoading] = useState(false);
  const [showSaved, setShowSaved] = useState(false);
  const fileInputRef = useRef<HTMLInputElement>(null);

  if (!note) return null;

  const handleMagicWand = async () => {
    if (!note.content) return;
    setIsAiLoading(true);
    try {
      const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });
      const response = await ai.models.generateContent({
        model: 'gemini-3-flash-preview',
        contents: `I am writing a professional note. Please refine and enhance the following text to be more structured, articulate, and insightful while maintaining its core meaning:\n\n${note.content}`,
        config: {
          systemInstruction: 'You are a professional editor. You transform messy thoughts into clear, organized, and impactful notes. Use headers and bullet points where appropriate.'
        }
      });
      const refined = response.text;
      if (refined) onUpdateNote(note.id, { content: refined });
    } catch (err) {
      console.error(err);
      alert('The Magic Wand is out of mana! Try again later.');
    } finally {
      setIsAiLoading(false);
    }
  };

  const saveNote = () => {
    setShowSaved(true);
    setTimeout(() => setShowSaved(false), 2000);
  };

  const handleImageUpload = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        const base64String = reader.result as string;
        const currentImages = note.images || [];
        onUpdateNote(note.id, { images: [...currentImages, base64String] });
      };
      reader.readAsDataURL(file);
    }
  };

  const removeImage = (index: number) => {
    const currentImages = note.images || [];
    const updatedImages = currentImages.filter((_, i) => i !== index);
    onUpdateNote(note.id, { images: updatedImages });
  };

  return (
    <div className="h-full flex flex-col bg-white rounded-[3rem] shadow-[0_40px_100px_rgba(0,0,0,0.04)] overflow-hidden border border-gray-50 animate-in zoom-in-95 duration-500">
      {/* Editor Header */}
      <div className="px-10 py-6 border-b border-gray-50 flex items-center justify-between shrink-0 bg-white">
        <div className="flex items-center gap-4">
          <button onClick={onBack} className="w-12 h-12 bg-gray-50 hover:bg-gray-100 rounded-2xl flex items-center justify-center text-gray-400 transition-all">
            <ChevronLeft size={24} strokeWidth={2.5} />
          </button>
          <div className="h-8 w-[1px] bg-gray-100 mx-2"></div>
          <button 
            onClick={saveNote}
            className="px-6 py-3 bg-[#2D3436] text-white rounded-2xl text-sm font-bold flex items-center gap-2 hover:bg-[#1e2324] transition-all shadow-lg shadow-gray-200"
          >
            {showSaved ? <CheckCircle2 size={18} /> : <Save size={18} />}
            <span>{showSaved ? 'Saved' : 'Save Changes'}</span>
          </button>
        </div>

        <div className="flex items-center gap-3">
          <button 
            onClick={handleMagicWand}
            disabled={isAiLoading}
            className={`px-6 py-3 bg-[#F3E5F5] text-[#7B1FA2] rounded-2xl text-sm font-black flex items-center gap-2 border border-[#F3E5F5]/50 transition-all hover:scale-105 active:scale-95 ${isAiLoading ? 'opacity-50 animate-pulse' : ''}`}
          >
            <Wand2 size={18} className={isAiLoading ? 'animate-spin' : ''} />
            <span>{isAiLoading ? 'Brewing...' : 'Magic Refine'}</span>
          </button>
          <button className="w-12 h-12 bg-gray-50 hover:bg-gray-100 rounded-2xl flex items-center justify-center text-gray-400 transition-all">
            <Share2 size={20} />
          </button>
        </div>
      </div>

      <div className="flex-1 overflow-y-auto px-10 py-12 md:px-24 scrollbar-hide bg-white">
        <div className="max-w-4xl mx-auto space-y-8">
          {/* Title Area - Forced White Background */}
          <input 
            type="text" 
            value={note.title}
            onChange={(e) => onUpdateNote(note.id, { title: e.target.value })}
            className="w-full text-5xl font-black text-[#2D3436] placeholder:text-gray-200 outline-none border-none tracking-tight leading-tight bg-white"
            placeholder="Name your spark..."
          />
          
          {/* Format Toolbar (Floating) */}
          <div className="sticky top-0 z-10 bg-white/80 backdrop-blur-md py-4 border-b border-gray-50 flex items-center gap-2">
            <button className="p-3 hover:bg-gray-50 rounded-2xl text-gray-400 hover:text-gray-700 transition-colors"><Bold size={20} /></button>
            <button className="p-3 hover:bg-gray-50 rounded-2xl text-gray-400 hover:text-gray-700 transition-colors"><Italic size={20} /></button>
            <div className="w-[1px] h-6 bg-gray-100 mx-2"></div>
            <button className="p-3 hover:bg-gray-50 rounded-2xl text-gray-400 hover:text-gray-700 transition-colors"><List size={20} /></button>
            <button className="p-3 hover:bg-gray-50 rounded-2xl text-gray-400 hover:text-gray-700 transition-colors"><Highlighter size={20} /></button>
            
            <div className="w-[1px] h-6 bg-gray-100 mx-2"></div>
            
            <input 
              type="file" 
              ref={fileInputRef} 
              className="hidden" 
              accept="image/*" 
              onChange={handleImageUpload} 
            />
            <button 
              onClick={() => fileInputRef.current?.click()}
              className="p-3 hover:bg-gray-50 rounded-2xl text-gray-400 hover:text-[#7B1FA2] transition-colors"
              title="Add Image"
            >
              <ImageIcon size={20} />
            </button>
            
            <div className="flex-1"></div>
            <button className="p-3 hover:bg-gray-50 rounded-2xl text-gray-300 transition-colors"><Maximize2 size={18} /></button>
          </div>

          {/* Image Display Grid */}
          {note.images && note.images.length > 0 && (
            <div className="grid grid-cols-2 md:grid-cols-3 gap-4 pb-4">
              {note.images.map((img, idx) => (
                <div key={idx} className="relative group aspect-video rounded-2xl overflow-hidden border border-gray-100 shadow-sm">
                  <img src={img} alt={`Attached ${idx}`} className="w-full h-full object-cover" />
                  <button 
                    onClick={() => removeImage(idx)}
                    className="absolute top-2 right-2 p-1.5 bg-black/50 text-white rounded-lg opacity-0 group-hover:opacity-100 transition-opacity hover:bg-rose-500"
                  >
                    <X size={14} />
                  </button>
                </div>
              ))}
            </div>
          )}

          {/* Editor Body - Forced White Background */}
          <textarea 
            value={note.content}
            onChange={(e) => onUpdateNote(note.id, { content: e.target.value })}
            className="w-full min-h-[600px] resize-none text-xl text-gray-600 placeholder:text-gray-200 outline-none border-none leading-relaxed font-medium bg-white"
            placeholder="Let your thoughts flow freely here..."
          />
        </div>
      </div>
    </div>
  );
};

export default NoteEditor;


import React, { useState } from 'react';
import { UserProfile } from '../types';
import { ChevronLeft, Camera, CheckCircle2, User, Mail, FileText } from 'lucide-react';

interface ProfileViewProps {
  profile: UserProfile;
  onUpdateProfile: (updates: Partial<UserProfile>) => void;
  onBack: () => void;
}

const ProfileView: React.FC<ProfileViewProps> = ({ profile, onUpdateProfile, onBack }) => {
  const [formData, setFormData] = useState(profile);
  const [isSaved, setIsSaved] = useState(false);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onUpdateProfile(formData);
    setIsSaved(true);
    setTimeout(() => setIsSaved(false), 2000);
  };

  return (
    <div className="max-w-3xl mx-auto animate-in fade-in slide-in-from-bottom-8 duration-700">
      <div className="mb-10 flex items-center gap-4">
        <button onClick={onBack} className="w-12 h-12 bg-white border border-gray-100 rounded-2xl flex items-center justify-center text-gray-400 hover:text-[#7B1FA2] transition-all shadow-sm">
          <ChevronLeft size={24} strokeWidth={2.5} />
        </button>
        <div>
          <h1 className="text-3xl font-black text-[#2D3436]">Profile Settings</h1>
          <p className="text-gray-400 font-medium">Manage how you appear in your workspaces.</p>
        </div>
      </div>

      <div className="bg-white rounded-[3rem] p-10 md:p-14 shadow-[0_40px_100px_rgba(0,0,0,0.04)] border border-gray-50">
        <form onSubmit={handleSubmit} className="space-y-10">
          {/* Avatar Section */}
          <div className="flex flex-col items-center">
            <div className="relative group">
              <div className="w-32 h-32 rounded-[2.5rem] bg-gradient-to-tr from-[#F3E5F5] to-[#E0F2F1] p-1 shadow-xl shadow-purple-100 overflow-hidden">
                <img src={formData.avatar} alt="Avatar" className="w-full h-full object-cover rounded-[2.2rem]" />
              </div>
              <button 
                type="button"
                onClick={() => setFormData({...formData, avatar: `https://picsum.photos/seed/${Math.random()}/200`})}
                className="absolute -bottom-2 -right-2 w-10 h-10 bg-[#2D3436] text-white rounded-xl flex items-center justify-center hover:scale-110 transition-transform shadow-lg"
              >
                <Camera size={18} />
              </button>
            </div>
            <p className="mt-4 text-xs font-black text-gray-300 uppercase tracking-widest">Click icon to shuffle avatar</p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
            <div className="space-y-2">
              <label className="text-xs font-black text-gray-400 uppercase tracking-widest ml-1 flex items-center gap-2">
                <User size={12} /> Full Name
              </label>
              <input 
                type="text" 
                value={formData.name}
                onChange={e => setFormData({...formData, name: e.target.value})}
                className="w-full bg-[#FAF9F6] border border-gray-100 rounded-2xl py-4 px-6 text-sm font-bold focus:bg-white focus:ring-4 focus:ring-[#F3E5F5]/30 focus:border-[#F3E5F5] transition-all outline-none"
                placeholder="Your name"
              />
            </div>

            <div className="space-y-2">
              <label className="text-xs font-black text-gray-400 uppercase tracking-widest ml-1 flex items-center gap-2">
                <Mail size={12} /> Email Address
              </label>
              <input 
                type="email" 
                value={formData.email}
                onChange={e => setFormData({...formData, email: e.target.value})}
                className="w-full bg-[#FAF9F6] border border-gray-100 rounded-2xl py-4 px-6 text-sm font-bold focus:bg-white focus:ring-4 focus:ring-[#F3E5F5]/30 focus:border-[#F3E5F5] transition-all outline-none"
                placeholder="your@email.com"
              />
            </div>
          </div>

          <div className="space-y-2">
            <label className="text-xs font-black text-gray-400 uppercase tracking-widest ml-1 flex items-center gap-2">
              <FileText size={12} /> Short Bio
            </label>
            <textarea 
              value={formData.bio}
              onChange={e => setFormData({...formData, bio: e.target.value})}
              className="w-full bg-[#FAF9F6] border border-gray-100 rounded-2xl py-4 px-6 text-sm font-bold focus:bg-white focus:ring-4 focus:ring-[#F3E5F5]/30 focus:border-[#F3E5F5] transition-all outline-none min-h-[120px] resize-none"
              placeholder="Tell us about yourself..."
            />
          </div>

          <button 
            type="submit"
            className="w-full py-5 bg-[#2D3436] text-white rounded-[1.5rem] font-black flex items-center justify-center gap-3 hover:bg-[#1e2324] hover:shadow-2xl hover:translate-y-[-2px] transition-all active:scale-95"
          >
            {isSaved ? <CheckCircle2 size={20} /> : null}
            <span>{isSaved ? 'Settings Applied' : 'Update Profile'}</span>
          </button>
        </form>
      </div>
    </div>
  );
};

export default ProfileView;



import React from 'react';
import { Space, UserProfile } from '../types';
import { 
  Plus, 
  Home, 
  LogOut, 
  Settings,
  UserEdit,
  User
} from 'lucide-react';

interface SidebarProps {
  spaces: Space[];
  profile: UserProfile;
  activeSpaceId: string | null;
  onSelectSpace: (id: string | null) => void;
  onCreateSpace: (name: string) => void;
  onCreateNote: () => void;
  onGoHome: () => void;
  onEditProfile: () => void;
  onLogout: () => void;
}

const Sidebar: React.FC<SidebarProps> = ({ 
  spaces, 
  profile,
  activeSpaceId, 
  onSelectSpace, 
  onCreateSpace, 
  onCreateNote, 
  onGoHome, 
  onEditProfile,
  onLogout 
}) => {
  return (
    <div className="w-20 lg:w-64 h-full bg-white border border-gray-100 rounded-[2.5rem] flex flex-col shadow-[0_20px_50px_rgba(0,0,0,0.03)] overflow-hidden transition-all duration-300">
      {/* Spacer where branding used to be */}
      <div className="p-8 pb-4"></div>

      {/* Main Action */}
      <div className="px-6 py-4">
        <button 
          onClick={onCreateNote}
          className="w-full aspect-square lg:aspect-auto lg:h-12 bg-[#2D3436] text-white rounded-2xl flex items-center justify-center gap-2 hover:bg-[#1e2324] hover:shadow-lg transition-all"
        >
          <Plus size={24} />
          <span className="hidden lg:block font-bold text-sm">New Note</span>
        </button>
      </div>

      {/* Navigation */}
      <nav className="flex-1 px-4 py-4 space-y-2 overflow-y-auto scrollbar-hide">
        <button 
          onClick={onGoHome}
          className={`w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 rounded-2xl transition-all ${!activeSpaceId ? 'bg-[#F3E5F5] text-[#7B1FA2]' : 'text-gray-400 hover:bg-gray-50'}`}
        >
          <Home size={20} strokeWidth={2.5} />
          <span className="hidden lg:block font-bold text-sm">Home</span>
        </button>

        <div className="hidden lg:block pt-6 pb-2">
          <p className="px-4 text-[10px] font-black text-gray-300 uppercase tracking-widest">Collections</p>
        </div>

        {spaces.map(space => (
          <button
            key={space.id}
            onClick={() => onSelectSpace(space.id)}
            className={`w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 rounded-2xl transition-all ${activeSpaceId === space.id ? 'bg-[#E0F2F1] text-[#00796B]' : 'text-gray-400 hover:bg-gray-50'}`}
          >
            <div className={`w-2.5 h-2.5 rounded-full ${space.color.split(' ')[0]} shadow-sm`}></div>
            <span className="hidden lg:block font-bold text-sm truncate">{space.name}</span>
          </button>
        ))}

        <button 
          onClick={() => { const n = prompt('Collection Name:'); if(n) onCreateSpace(n); }}
          className="w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 text-gray-300 hover:text-[#7B1FA2] transition-colors"
        >
          <Plus size={20} />
          <span className="hidden lg:block font-bold text-sm">Add New</span>
        </button>
      </nav>

      {/* Footer / Profile Action */}
      <div className="p-4 border-t border-gray-50 space-y-2">
        <button 
          onClick={onEditProfile}
          className="w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 text-gray-400 hover:bg-gray-50 rounded-2xl transition-all"
        >
          <User size={20} />
          <span className="hidden lg:block font-bold text-sm">Edit Profile</span>
        </button>
        <button className="w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 text-gray-400 hover:bg-gray-50 rounded-2xl transition-all">
          <Settings size={20} />
          <span className="hidden lg:block font-bold text-sm">Settings</span>
        </button>
        <button 
          onClick={onLogout}
          className="w-full p-3 lg:px-4 lg:py-3 flex items-center justify-center lg:justify-start gap-3 text-rose-300 hover:bg-rose-50 hover:text-rose-500 rounded-2xl transition-all"
        >
          <LogOut size={20} />
          <span className="hidden lg:block font-bold text-sm">Logout</span>
        </button>
      </div>
    </div>
  );
};

export default Sidebar;
