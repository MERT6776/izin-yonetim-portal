<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>İzin Yönetim Portalı - Burhan Biliktü</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
html, body { height: 100%; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; }
.app { min-height: 100vh; background: linear-gradient(135deg, #f0fdf4 0%, #f0f9ff 100%); display: flex; flex-direction: column; }
.login-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; padding: 1.5rem; }
.login-card { background: white; border-radius: 16px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); padding: 2rem 1.5rem; width: 100%; max-width: 420px; border: 1px solid #e0f2fe; }
.login-header { text-align: center; margin-bottom: 2rem; }
.logo-icon { width: 56px; height: 56px; background: linear-gradient(135deg, #0f766e 0%, #06b6d4 100%); border-radius: 12px; display: flex; align-items: center; justify-content: center; margin: 0 auto 1rem; font-size: 28px; }
h1 { font-size: 24px; font-weight: 600; color: #0f766e; margin-bottom: 0.5rem; }
.form-group { margin-bottom: 1.5rem; }
.form-group label { display: block; font-size: 13px; font-weight: 500; color: #1e293b; margin-bottom: 0.5rem; }
.form-group input { width: 100%; padding: 0.875rem; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 16px; font-family: inherit; }
.form-group input:focus { outline: none; border-color: #0f766e; box-shadow: 0 0 0 3px rgba(15, 118, 110, 0.1); }
.login-btn { width: 100%; padding: 1rem; background: linear-gradient(135deg, #0f766e 0%, #06b6d4 100%); color: white; border: none; border-radius: 8px; font-size: 15px; font-weight: 600; cursor: pointer; }
.login-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 16px rgba(15, 118, 110, 0.3); }
.test-info { background: #f0fdf4; border: 1px solid #bbf7d0; border-radius: 8px; padding: 1rem; margin-top: 1.5rem; font-size: 12px; color: #166534; line-height: 1.6; }
.error-msg { background: #fee2e2; border: 1px solid #fca5a5; border-radius: 8px; padding: 0.75rem; margin-bottom: 1rem; font-size: 12px; color: #dc2626; display: none; }
.error-msg.show { display: block; }
.dashboard { flex: 1; display: flex; flex-direction: column; padding: 1rem; }
.header { display: flex; justify-content: space-between; align-items: flex-start; gap: 1rem; margin-bottom: 1.5rem; }
.header h2 { font-size: 18px; font-weight: 600; color: #0f766e; }
.logout-btn { padding: 0.5rem 1rem; background: white; border: 1px solid #cbd5e1; border-radius: 6px; cursor: pointer; font-size: 12px; color: #1e293b; font-weight: 500; }
.tabs { display: flex; gap: 0.5rem; margin-bottom: 1.5rem; background: white; padding: 0.5rem; border-radius: 12px; box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
.tab { flex: 1; padding: 0.75rem; background: transparent; border: none; border-radius: 8px; font-size: 13px; font-weight: 600; color: #64748b; cursor: pointer; }
.tab.active { background: linear-gradient(135deg, #0f766e 0%, #06b6d4 100%); color: white; }
.content { flex: 1; overflow-y: auto; padding-bottom: 1rem; }
.card { background: white; border-radius: 14px; box-shadow: 0 4px 12px rgba(0,0,0,0.08); border: 1px solid #e0f2fe; padding: 1.5rem; margin-bottom: 1rem; }
.card-title { font-size: 14px; font-weight: 600; color: #0f766e; margin-bottom: 1rem; }
.summary { background: linear-gradient(135deg, #f0fdf4 0%, #e0f2fe 100%); border: 1px solid #bbf7d0; border-radius: 12px; padding: 1.5rem; text-align: center; margin-bottom: 1rem; }
.summary-label { font-size: 11px; font-weight: 600; color: #059669; text-transform: uppercase; margin-bottom: 0.75rem; }
.summary-number { font-size: 52px; font-weight: 700; background: linear-gradient(135deg, #0f766e 0%, #06b6d4 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; margin-bottom: 0.5rem; }
.info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-bottom: 1rem; }
.info-card { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1rem; }
.info-card-label { font-size: 10px; font-weight: 600; color: #64748b; text-transform: uppercase; margin-bottom: 0.5rem; }
.info-card-value { font-size: 14px; font-weight: 600; color: #0f766e; }
.btn { flex: 1; padding: 1rem; border-radius: 8px; border: 1px solid #cbd5e1; font-size: 13px; font-weight: 600; cursor: pointer; min-height: 48px; display: flex; align-items: center; justify-content: center; }
.btn-primary { background: linear-gradient(135deg, #0f766e 0%, #06b6d4 100%); color: white; border: none; }
.hidden { display: none; }
</style>
</head>
<body>
<div id="app"></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/react-dom.production.min.js"></script>
<script>
const { useState } = React;
function HRPortal() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [showError, setShowError] = useState(false);
  const [activeTab, setActiveTab] = useState('dashboard');
  const employees = {
    'burhan671819': { name: 'BURHAN BİLİKTÜ', title: 'KALİTE GÜVENCE & KONTROL TEKNİKERİ', totalLeave: 27.5, pazarIzinleri: 17, resmiTatil: 10.5, password: '181920' }
  };
  const handleLogin = () => {
    const emp = employees[username.toLowerCase()];
    if (emp && emp.password === password) {
      setIsLoggedIn(true);
      setShowError(false);
    } else {
      setShowError(true);
      setTimeout(() => setShowError(false), 3000);
    }
  };
  const handleLogout = () => {
    setIsLoggedIn(false);
    setUsername('');
    setPassword('');
    setActiveTab('dashboard');
  };
  const employee = employees[username.toLowerCase()];
  if (!isLoggedIn) {
    return React.createElement('div', { className: 'app' },
      React.createElement('div', { className: 'login-screen' },
        React.createElement('div', { className: 'login-card' },
          React.createElement('div', { className: 'login-header' },
            React.createElement('div', { className: 'logo-icon' }, '📋'),
            React.createElement('h1', null, 'İzin Yönetim'),
            React.createElement('p', null, 'HR Mobil Portal')
          ),
          showError && React.createElement('div', { className: 'error-msg show' }, '✗ Kullanıcı adı veya şifre yanlış'),
          React.createElement('div', { className: 'form-group' },
            React.createElement('label', null, 'Kullanıcı Adı'),
            React.createElement('input', { type: 'text', value: username, onChange: (e) => setUsername(e.target.value), onKeyPress: (e) => e.key === 'Enter' && handleLogin(), placeholder: 'BURHAN671819' })
          ),
          React.createElement('div', { className: 'form-group' },
            React.createElement('label', null, 'Şifre'),
            React.createElement('input', { type: 'password', value: password, onChange: (e) => setPassword(e.target.value), onKeyPress: (e) => e.key === 'Enter' && handleLogin(), placeholder: '••••••' })
          ),
          React.createElement('button', { className: 'login-btn', onClick: handleLogin }, 'Giriş Yap'),
          React.createElement('div', { className: 'test-info' }, '📊 Excel Verileriyle Giriş\n', React.createElement('strong', null, 'Kullanıcı:'), ' BURHAN671819\n', React.createElement('strong', null, 'Şifre:'), ' 181920')
        )
      )
    );
  }
  return React.createElement('div', { className: 'app' },
    React.createElement('div', { className: 'dashboard' },
      React.createElement('div', { className: 'header' },
        React.createElement('div', null,
          React.createElement('h2', null, 'Hoşgeldiniz'),
          React.createElement('p', null, employee.name)
        ),
        React.createElement('button', { className: 'logout-btn', onClick: handleLogout }, 'Çıkış Yap')
      ),
      React.createElement('div', { className: 'tabs' },
        React.createElement('button', { className: `tab ${activeTab === 'dashboard' ? 'active' : ''}`, onClick: () => setActiveTab('dashboard') }, '📊 Özet'),
        React.createElement('button', { className: `tab ${activeTab === 'details' ? 'active' : ''}`, onClick: () => setActiveTab('details') }, '📋 Detaylar')
      ),
      React.createElement('div', { className: 'content' },
        activeTab === 'dashboard' && React.createElement(React.Fragment, null,
          React.createElement('div', { className: 'summary' },
            React.createElement('div', { className: 'summary-label' }, '✨ KALAN İZİN HAKKI'),
            React.createElement('div', { className: 'summary-number' }, employee.totalLeave),
            React.createElement('div', { className: 'summary-label', style: { fontSize: '12px', marginTop: '0.5rem' } }, 'gün')
          ),
          React.createElement('div', { className: 'card' },
            React.createElement('div', { className: 'card-title' }, '📊 İzin Dağılımı'),
            React.createElement('div', { className: 'info-grid' },
              React.createElement('div', { className: 'info-card' },
                React.createElement('div', { className: 'info-card-label' }, 'Pazar İzinleri'),
                React.createElement('div', { className: 'info-card-value' }, employee.pazarIzinleri + ' gün')
              ),
              React.createElement('div', { className: 'info-card' },
                React.createElement('div', { className: 'info-card-label' }, 'Resmi Tatil'),
                React.createElement('div', { className: 'info-card-value' }, employee.resmiTatil + ' gün')
              )
            )
          )
        ),
        activeTab === 'details' && React.createElement('div', { className: 'card' },
          React.createElement('div', { className: 'card-title' }, '📞 İtiraz'),
          React.createElement('button', { className: 'btn btn-primary', onClick: () => window.open('https://wa.me/905551234567?text=' + encodeURIComponent('İzin talebim hakkında iletişime geçmek istiyorum'), '_blank') }, '💬 WhatsApp')
        )
      )
    )
  );
}
ReactDOM.render(React.createElement(HRPortal), document.getElementById('app'));
</script>
</body>
</html>
