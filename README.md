
import React, { useState } from 'react';

interface NavbarProps {
  activeSection: string;
  onNavClick: (id: string) => void;
}

const Navbar: React.FC<NavbarProps> = ({ activeSection, onNavClick }) => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [showCopied, setShowCopied] = useState(false);

  const navLinks = [
    { id: 'home', label: 'בית' },
    { id: 'services', label: 'התמחויות' },
    { id: 'faq', label: 'שאלות נפוצות' },
    { id: 'about', label: 'אודות' },
    { id: 'contact', label: 'צור קשר' },
  ];

  const handleShare = async () => {
    const shareData = {
      title: 'מצובה ביטוחים - אורי קדושי',
      text: 'תראה את האתר החדש של מצובה ביטוחים!',
      url: window.location.href,
    };

    if (navigator.share) {
      try {
        await navigator.share(shareData);
      } catch (err) {
        console.log('Share failed', err);
      }
    } else {
      navigator.clipboard.writeText(window.location.href);
      setShowCopied(true);
      setTimeout(() => setShowCopied(false), 2000);
    }
  };

  return (
    <nav className="sticky top-0 z-50 bg-white/95 backdrop-blur-md border-b border-mazuba-blue/10">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex justify-between h-24">
          <div className="flex items-center">
            <div 
              className="flex-shrink-0 flex items-center cursor-pointer group" 
              onClick={() => onNavClick('home')}
            >
              <div className="w-12 h-12 bg-mazuba-blue rounded-lg flex items-center justify-center text-white font-bold text-2xl ml-3 shadow-lg group-hover:bg-mazuba-orange transition-colors">
                מ
              </div>
              <div className="flex flex-col">
                <span className="text-2xl font-black text-mazuba-blue tracking-tight leading-none">מצובה ביטוחים</span>
                <span className="text-sm font-bold text-mazuba-orange leading-none mt-1">אורי קדושי סוכן ביטוח</span>
              </div>
            </div>
          </div>
          
          <div className="hidden md:flex items-center space-x-6 space-x-reverse">
            {navLinks.map((link) => (
              <button
                key={link.id}
                onClick={() => onNavClick(link.id)}
                className={`text-base font-bold transition-all duration-200 px-2 py-1 ${
                  activeSection === link.id 
                  ? 'text-mazuba-blue border-b-4 border-mazuba-orange' 
                  : 'text-gray-600 hover:text-mazuba-blue'
                }`}
              >
                {link.label}
              </button>
            ))}
            
            <button 
              onClick={handleShare}
              className="relative p-2 text-mazuba-blue hover:text-mazuba-orange transition-colors flex flex-col items-center"
              title="שתפו את האתר לפידבק"
            >
              <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" />
              </svg>
              <span className="text-[10px] font-bold">שיתוף</span>
              {showCopied && (
                <span className="absolute -bottom-8 bg-black text-white text-[10px] px-2 py-1 rounded">הלינק הועתק!</span>
              )}
            </button>

            <button 
              onClick={() => onNavClick('contact')}
              className="bg-mazuba-orange text-white px-6 py-2.5 rounded-full font-black hover:bg-mazuba-blue transition-all shadow-lg hover:shadow-mazuba-orange/20 transform active:scale-95"
            >
              לייעוץ עם אורי
            </button>
          </div>

          <div className="md:hidden flex items-center">
            <button
              onClick={() => setIsMenuOpen(!isMenuOpen)}
              className="text-mazuba-blue hover:text-mazuba-orange focus:outline-none"
            >
              <svg className="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                {isMenuOpen ? (
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
                ) : (
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
                )}
              </svg>
            </button>
          </div>
        </div>
      </div>

      {isMenuOpen && (
        <div className="md:hidden bg-white border-t border-gray-100 py-4 shadow-2xl">
          <div className="px-2 pt-2 pb-3 space-y-1 sm:px-3 text-right">
            {navLinks.map((link) => (
              <button
                key={link.id}
                onClick={() => {
                  onNavClick(link.id);
                  setIsMenuOpen(false);
                }}
                className={`block w-full text-right px-4 py-3 text-lg font-bold rounded-md ${
                  activeSection === link.id ? 'bg-mazuba-lightBlue text-mazuba-blue' : 'text-gray-600 hover:bg-gray-50'
                }`}
              >
                {link.label}
              </button>
            ))}
            <button 
              onClick={handleShare}
              className="flex items-center justify-end w-full px-4 py-3 text-lg font-bold text-mazuba-blue"
            >
              <span>שתפו את האתר לפידבק</span>
              <svg className="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" />
              </svg>
            </button>
            <div className="pt-4 px-4">
               <button 
                onClick={() => onNavClick('contact')}
                className="w-full bg-mazuba-orange text-white py-4 rounded-xl font-black shadow-lg"
              >
                לייעוץ אישי עכשיו
              </button>
            </div>
          </div>
        </div>
      )}
    </nav>
  );
};

export default Navbar;
