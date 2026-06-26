# AndoBAmux_krypto_wallet_mx
Billetera sin custodia , al ojo del amo engorda en caballo , sincroniza para que nadie sepa de tus llaves 
# 📱 GUÍA COMPLETA: Generar APK ANDOBAMUX en Termux
### Realme 12 5G · Android · KONRE v21

---

## 📋 Antes de empezar — Requisitos

| Requisito | ¿Dónde obtenerlo? |
|-----------|-------------------|
| Termux instalado | F-Droid (el APK que ya tienes) ✅ |
| Android 8.0+ | Realme 12 5G ✅ |
| ~3 GB espacio libre | Ajustes → Almacenamiento |
| Conexión a internet | WiFi o datos |

---

## 🔑 PASO 0 — Instalar Termux desde F-Droid

> Ya tienes el APK `F-Droid.apk` descargado.

```
1. Abre Ajustes → Seguridad → Fuentes desconocidas → ACTIVAR
2. Abre el F-Droid.apk con el gestor de archivos
3. Presiona INSTALAR → ABRIR
4. En F-Droid busca "Termux" → INSTALAR
5. Abre Termux
```

---

## 🛠 PASO 1 — Configurar Termux (primera vez)

Abre Termux y ejecuta estos comandos **uno por uno**:

```bash
# 1.1 Dar permiso de almacenamiento
termux-setup-storage
# → Aparece un diálogo → Pulsa PERMITIR

# 1.2 Actualizar repositorios
pkg update -y

# 1.3 Actualizar paquetes
pkg upgrade -y
```

**Si te pregunta algo, presiona `Y` y Enter.**

---

## 📦 PASO 2 — Instalar herramientas base

```bash
# Instalar todo en un solo comando:
pkg install -y python nodejs-lts git curl wget zip unzip

# Verificar instalaciones:
python --version    # → Python 3.12.x
node --version      # → v20.x.x
git --version       # → git 2.x.x
```

---

## 🔐 PASO 3 — Instalar dependencias Python (KONRE)

```bash
# Instalar pip primero si no está
pkg install -y python-pip

# Instalar librerías KONRE:
pip install --break-system-packages \
  cryptography \
  requests \
  rich \
  web3 \
  qrcode[pil]

# Verificar:
python -c "from cryptography.hazmat.primitives.ciphers.aead import AESGCM; print('OK')"
# → OK
```

---

## 📁 PASO 4 — Crear carpeta del proyecto

```bash
# Crear estructura de directorios
mkdir -p ~/konre/{src,termux,config,build}
mkdir -p ~/storage/downloads

# Ver que se creó:
ls ~/konre/
# → build  config  src  termux
```

---

## 📥 PASO 5 — Copiar archivos del proyecto

### Opción A: Desde Downloads (recomendado)

```bash
# Si descargaste el ZIP andobamux_konre_v21.zip:
cd ~/storage/downloads
ls   # → verifica que está el ZIP

# Descomprimir:
unzip andobamux_konre_v21.zip -d ~/konre/
ls ~/konre/
# → andobamux/  (con todos los módulos)
```

### Opción B: Crear archivos manualmente

```bash
# Crear termux_sync.py (pega el contenido del archivo):
nano ~/konre/termux/termux_sync.py
# Ctrl+X → Y → Enter para guardar

# Crear build_apk.sh:
nano ~/konre/termux/build_apk.sh
# Pega el contenido → Ctrl+X → Y → Enter
```

---

## ⚙️ PASO 6 — Configurar variables de entorno

```bash
# Abrir el archivo de configuración:
nano ~/.bashrc

# Agregar estas líneas al final:
export KONRE_API="https://andobamux-gxzaxdxx.manus.space/api"
export KONRE_DOMAIN="andobamux-gxzaxdxx.manus.space"
# export KONRE_JWT="<pega_tu_token_jwt_aquí>"
# export KONRE_AES_KEY="<pega_tu_clave_hex_64_chars>"

# Guardar: Ctrl+X → Y → Enter

# Recargar configuración:
source ~/.bashrc

# Verificar:
echo $KONRE_API
# → https://andobamux-gxzaxdxx.manus.space/api
```

---

## 🔑 PASO 7 — Generar tu clave AES-256

```bash
# Generar clave aleatoria de 256 bits (64 hex chars):
python3 -c "import os; print(os.urandom(32).hex())"
# Ejemplo de salida:
# a3f8b2c1d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0

# COPIA esa clave y pégala en ~/.bashrc:
# export KONRE_AES_KEY="a3f8b2c1d4e5f6a7..."

# ⚠ GUARDA ESTA CLAVE EN UN LUGAR SEGURO
# Si la pierdes, no podrás descifrar tus wallets guardadas
```

---

## 🌐 PASO 8 — Probar conexión con el servidor

```bash
# Test rápido:
curl -s https://andobamux-gxzaxdxx.manus.space/api/health | python3 -m json.tool

# Si el servidor responde:
# {
#   "status": "ok",
#   "version": "21.0.0",
#   ...
# }

# Si hay error de conexión:
ping andobamux-gxzaxdxx.manus.space
# Verifica tu WiFi o datos
```

---

## 🤖 PASO 9 — Ejecutar termux_sync.py

```bash
cd ~/konre/termux
python3 termux_sync.py

# Deberías ver:
# ╔══════════════════════════════╗
# ║ ANDOBAMUX WALLET_MX · KONRE ║
# ╚══════════════════════════════╝
#
# ── MENÚ TERMUX SYNC ──
#   1 · Sincronizar saldos
#   2 · Detectar tokens nuevos
#   3 · Exportar CSV transacciones
#   4 · Estado del servidor
#   5 · Salir
# >
```

---

## 📱 PASO 10 — GENERAR APK (3 métodos)

---

### 🅰 MÉTODO 1: PWA (MÁS FÁCIL — Recomendado)

> No requiere compilación. Funciona inmediatamente.

```
1. Abre Chrome en tu Realme 12 5G
2. Ve a: https://andobamux-gxzaxdxx.manus.space
3. Toca el menú ⋮ (tres puntos)
4. Selecciona "Añadir a pantalla de inicio"
5. Escribe nombre: "ANDOBAMUX"
6. Toca "AGREGAR"
7. ✅ Listo — icono en tu pantalla de inicio
```

---

### 🅱 MÉTODO 2: APK con Script Automático

```bash
# Dar permisos de ejecución:
chmod +x ~/konre/termux/build_apk.sh

# Ejecutar (puede tardar 10-30 minutos):
bash ~/konre/termux/build_apk.sh

# El script hará automáticamente:
# ✓ Verificar Termux y storage
# ✓ Instalar paquetes necesarios
# ✓ Crear estructura APK
# ✓ Generar AndroidManifest.xml
# ✓ Compilar MainActivity.java
# ✓ Crear KonreBridge.java
# ✓ Empaquetar assets web
# ✓ Intentar compilación con Gradle
# ✓ Si falla → genera PWA + ZIP de fuentes

# APK generada en:
ls ~/storage/downloads/ANDOBAMUX_WALLET_*.apk
```

---

### 🅲 MÉTODO 3: APK Manual paso a paso (control total)

#### 3.1 — Instalar Java y herramientas de compilación

```bash
pkg install -y openjdk-17
java -version
# → openjdk 17.x.x

# Instalar build tools:
pkg install -y gradle aapt
gradle --version
# → Gradle 8.x
```

#### 3.2 — Descargar Android SDK (Build Tools)

```bash
# Crear directorio SDK:
mkdir -p ~/android-sdk/build-tools

# Descargar build-tools para ARM64:
cd ~/android-sdk
wget -q "https://dl.google.com/android/repository/build-tools_r34-linux.zip" -O build-tools.zip
unzip -q build-tools.zip
mv android-14 build-tools/34.0.0

# Agregar al PATH:
echo 'export ANDROID_HOME=$HOME/android-sdk' >> ~/.bashrc
echo 'export PATH=$PATH:$ANDROID_HOME/build-tools/34.0.0' >> ~/.bashrc
source ~/.bashrc
```

#### 3.3 — Crear proyecto Android

```bash
mkdir -p ~/konre_apk/{src/main/{java/mx/andobamux/wallet,assets/www,res/{values,drawable,xml}},gradle/wrapper}
cd ~/konre_apk
```

#### 3.4 — Crear AndroidManifest.xml

```bash
cat > src/main/AndroidManifest.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="mx.andobamux.wallet"
    android:versionCode="21"
    android:versionName="21.0.0">
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.CAMERA"/>
    <application
        android:label="ANDOBAMUX"
        android:theme="@style/AppTheme"
        android:hardwareAccelerated="true"
        android:allowBackup="false">
        <activity android:name=".MainActivity"
            android:exported="true"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
EOF
echo "✓ Manifest creado"
```

#### 3.5 — Crear MainActivity.java

```bash
cat > src/main/java/mx/andobamux/wallet/MainActivity.java << 'EOF'
package mx.andobamux.wallet;
import android.app.Activity;
import android.os.Bundle;
import android.webkit.*;
import android.view.*;
import android.graphics.Color;

public class MainActivity extends Activity {
    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().setStatusBarColor(Color.parseColor("#0A0A14"));
        webView = new WebView(this);
        setContentView(webView);
        WebSettings ws = webView.getSettings();
        ws.setJavaScriptEnabled(true);
        ws.setDomStorageEnabled(true);
        ws.setLoadWithOverviewMode(true);
        ws.setUseWideViewPort(true);
        webView.addJavascriptInterface(new KonreBridge(this), "KonreBridge");
        webView.loadUrl("https://andobamux-gxzaxdxx.manus.space");
    }

    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) webView.goBack();
        else super.onBackPressed();
    }
}
EOF
echo "✓ MainActivity creada"
```

#### 3.6 — Crear res/values/styles.xml

```bash
cat > src/main/res/values/styles.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" parent="android:Theme.Material.NoActionBar">
        <item name="android:windowBackground">#0A0A14</item>
        <item name="android:colorPrimary">#00FF88</item>
        <item name="android:statusBarColor">#0A0A14</item>
        <item name="android:navigationBarColor">#0A0A14</item>
    </style>
</resources>
EOF
echo "✓ Styles creado"
```

#### 3.7 — Crear build.gradle

```bash
cat > build.gradle << 'EOF'
plugins { id 'com.android.application' }
android {
    namespace 'mx.andobamux.wallet'
    compileSdk 34
    defaultConfig {
        applicationId "mx.andobamux.wallet"
        minSdk 26
        targetSdk 34
        versionCode 21
        versionName "21.0.0"
    }
    buildTypes {
        release { minifyEnabled false }
    }
}
dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
}
EOF
echo "✓ Gradle config creado"
```

#### 3.8 — Compilar APK

```bash
cd ~/konre_apk

# Compilar:
gradle assembleRelease

# Si tiene éxito:
ls build/outputs/apk/release/
# → app-release-unsigned.apk

# Copiar a Downloads:
cp build/outputs/apk/release/app-release-unsigned.apk \
   ~/storage/downloads/ANDOBAMUX_v21_unsigned.apk

echo "✓ APK generada en Downloads"
```

#### 3.9 — Firmar APK (necesario para instalar)

```bash
# Generar keystore (solo la primera vez):
keytool -genkeypair -v \
  -keystore ~/konre.keystore \
  -alias konre \
  -keyalg RSA -keysize 2048 \
  -validity 10000 \
  -storepass mipassword \
  -keypass mipassword \
  -dname "CN=KONRE,O=ANDOBAMUX,C=MX"

# Firmar APK:
apksigner sign \
  --ks ~/konre.keystore \
  --ks-pass pass:mipassword \
  --ks-key-alias konre \
  --out ~/storage/downloads/ANDOBAMUX_v21.apk \
  ~/storage/downloads/ANDOBAMUX_v21_unsigned.apk

echo "✓ APK firmada: ~/storage/downloads/ANDOBAMUX_v21.apk"
```

#### 3.10 — Instalar APK en el dispositivo

```bash
# Opción A: Instalar con adb (si tienes otro dispositivo):
adb install ~/storage/downloads/ANDOBAMUX_v21.apk

# Opción B: Abrir con gestor de archivos:
termux-open ~/storage/downloads/ANDOBAMUX_v21.apk

# Opción C: Notificación para abrir desde Downloads:
termux-notification \
  --title "ANDOBAMUX APK lista para instalar" \
  --content "Ve a Downloads y toca ANDOBAMUX_v21.apk" \
  --priority high

# ⚠ Asegúrate de tener activado:
# Ajustes → Aplicaciones → Instalar apps desconocidas → Termux → PERMITIR
```

---

## ✅ PASO 11 — Verificar instalación

```bash
# Test final — abrir la app:
am start -n mx.andobamux.wallet/.MainActivity
# → La app debe abrirse mostrando la wallet

# Ver logs de la app:
logcat | grep andobamux
```

---

## 🔄 PASO 12 — Configurar alias y acceso rápido

```bash
# Agregar aliases útiles:
cat >> ~/.bashrc << 'EOF'

# ── KONRE ALIASES ──
alias konre="python3 ~/konre/termux/termux_sync.py"
alias konre-build="bash ~/konre/termux/build_apk.sh"
alias konre-status="curl -s https://andobamux-gxzaxdxx.manus.space/api/health | python3 -m json.tool"
alias konre-log="tail -f ~/.konre/sync.log"
alias konre-key="python3 -c \"import os; print(os.urandom(32).hex())\""
EOF

source ~/.bashrc

# Ahora puedes usar:
konre           # → abre el menú de sincronización
konre-status    # → ver estado del servidor
konre-log       # → ver logs en tiempo real
konre-key       # → generar nueva clave AES
```

---

## 🔧 Solución de problemas frecuentes

### Error: `pkg: command not found`
```bash
export PATH=$PATH:/data/data/com.termux/files/usr/bin
```

### Error: `pip install` falla con permisos
```bash
pip install --break-system-packages cryptography rich requests
```

### Error: `JAVA_HOME not set`
```bash
export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))
export PATH=$PATH:$JAVA_HOME/bin
```

### Error: `gradle: not found`
```bash
pkg install gradle
# O instalar manualmente:
wget https://services.gradle.org/distributions/gradle-8.5-bin.zip
unzip gradle-8.5-bin.zip -d ~/gradle
export PATH=$PATH:~/gradle/gradle-8.5/bin
```

### Error: `No module named 'cryptography'`
```bash
pip install --break-system-packages --upgrade cryptography
```

### APK no instala: "App no instalada"
```bash
# Verificar que la APK está firmada:
apksigner verify ~/storage/downloads/ANDOBAMUX_v21.apk
# Debe decir: Verified using v2 scheme

# Si está dañada, regenerar:
bash ~/konre/termux/build_apk.sh
```

### La app carga pero no conecta
```bash
# Verificar conexión:
curl https://andobamux-gxzaxdxx.manus.space
# Verificar DNS:
nslookup andobamux-gxzaxdxx.manus.space
```

---

## 📊 Resumen de comandos principales

```bash
# Primera instalación (ejecutar una sola vez):
termux-setup-storage && pkg update -y && pkg upgrade -y
pkg install -y python nodejs-lts git curl zip
pip install --break-system-packages cryptography requests rich web3

# Generar clave AES:
python3 -c "import os; print(os.urandom(32).hex())"

# Construir APK:
bash ~/konre/termux/build_apk.sh

# Usar KONRE CLI:
konre

# Ver estado:
konre-status
```

---

## 🗺 Resumen visual del flujo

```
Termux instalado
    ↓
termux-setup-storage (permisos)
    ↓
pkg install python nodejs git
    ↓
pip install cryptography rich web3
    ↓
Configurar KONRE_JWT + KONRE_AES_KEY en ~/.bashrc
    ↓
bash build_apk.sh
    ↓
    ├─ ✅ Gradle OK → APK firmada en Downloads
    └─ ⚠ Sin Gradle → PWA en Chrome (igualmente funcional)
         ↓
Instalar APK o agregar PWA a pantalla de inicio
         ↓
✅ ANDOBAMUX WALLET_MX funcionando en tu Realme 12 5G
```

---

*ANDOBAMUX WALLET_MX · KONRE v21 · 🇲🇽 México · Termux · Realme 12 5G*
# ANDOBAMUX WALLET_MX — KONRE v21
## Documentación Técnica Completa

**Dominio:** `andobamux-gxzaxdxx.manus.space`  
**Versión:** 21.0.0  
**Plataforma:** Web + Android (Termux) + APK  
**Idioma:** Español México 🇲🇽  

---

## Arquitectura General

```
┌─────────────────────────────────────────────────────────┐
│                  ANDOBAMUX WALLET_MX                     │
├──────────────┬──────────────────┬───────────────────────┤
│   WEB APP    │   ANDROID APK    │       TERMUX           │
│  (React/Vite)│  (WebView/TWA)   │     (Python CLI)      │
│              │                  │                        │
│  Dashboard   │  KonreApp.java   │   termux_sync.py      │
│  Wallets     │  MainActivity    │   AES-256-GCM          │
│  Tokens      │  KonreBridge     │   Web3.py / CCXT      │
│  Autopilot   │  UsbOtgReceiver  │   USB-OTG Sign        │
│  Stripe      │                  │                        │
└──────┬───────┴─────────┬────────┴────────────┬──────────┘
       │                 │                     │
       └────────────┬────┘              AES cifrado
                    │                  JWT autenticado
              ┌─────▼─────┐
              │  API REST  │  https://andobamux-gxzaxdxx.manus.space
              │  (backend) │
              └─────┬─────┘
                    │
              ┌─────▼─────┐
              │  MySQL DB  │  (Drizzle ORM)
              └───────────┘
```

---

## Base de Datos (MySQL / Drizzle ORM)

### Tablas

| Tabla | Descripción |
|-------|-------------|
| `users` | Usuarios con AES key hash y entorno (web/termux) |
| `wallets` | Direcciones públicas por red (nunca claves privadas) |
| `tokens` | Tokens detectados con balances en USD y MXN |
| `transactions` | Historial completo (send/receive/swap/stake/trade) |
| `autopilot_operations` | Decisiones del LLM con reasoning en español |
| `notifications` | Centro de alertas (6 tipos) |
| `user_settings` | Config por usuario (redes, autopilot, moneda) |
| `stripe_transactions` | Pagos MXN via Stripe/OXXO/SPEI |

### Migración aplicada
```sql
-- Archivo: 0001_sloppy_justin_hammer.sql
-- Estado: ✓ Aplicada
ALTER TABLE users ADD encryptionKey text;
ALTER TABLE users ADD environment enum('web','termux') DEFAULT 'web';
```

---

## Seguridad

### Principios
- ⚠ **Claves privadas NUNCA en la web** — solo en Termux
- ⚠ **USB-OTG** es el único canal de firma: `Termux → USB → Web`
- ⚠ **AES-256-GCM** diferenciada por usuario y entorno
- ⚠ **Logs sin datos sensibles** — solo metadata

### Cifrado AES-256-GCM
```python
# Cifrar (en Termux)
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
key = bytes.fromhex(os.getenv("KONRE_AES_KEY"))  # 64 hex = 32 bytes
nonce = os.urandom(12)
aesgcm = AESGCM(key)
ct = aesgcm.encrypt(nonce, plaintext.encode(), None)
```

### Clave pública RSA
```
Archivo: log_list.pub
Algoritmo: RSA-4096 (PKCS#8)
Uso: Verificación de firma de transacciones USB-OTG
```

### JWT Tokens
```
KONRE_JWT     → Autenticación web/API (24h)
termuxSyncToken → Sincronización Termux (720h = 30 días)
```

---

## Módulos React (Frontend)

### Archivos generados

| Archivo | Descripción | Pestañas |
|---------|-------------|----------|
| `andobamux_wallet.jsx` | Dashboard principal | ◈ INICIO, ⬡ WALLETS, ◎ TOKENS, ⇄ HISTORIAL, ⚡ AUTOPILOT |
| `andobamux_modules.jsx` | Módulos adicionales | 🔔 ALERTAS, 💳 PAGOS, ⚡ USB-OTG, ⚙ CONFIG |

### Paleta de colores (index.css)
```css
--bg:       #0A0A14  /* Fondo oscuro */
--green:    #00FF88  /* Neón verde principal */
--teal:     #00C9A7  /* Teal secundario */
--red:      #FF4466  /* Alertas/envíos */
--yellow:   #FFD700  /* Warnings/pendiente */
--text:     #B0FFD8  /* Texto principal */
```

---

## Termux — Módulo Python

### Instalación rápida
```bash
# En Termux (Realme 12 5G)
pkg update && pkg install python git
pip install cryptography requests rich web3 --break-system-packages

# Configurar variables
export KONRE_API="https://andobamux-gxzaxdxx.manus.space/api"
export KONRE_JWT="<tu_jwt>"
export KONRE_AES_KEY="<64_hex>"

# Ejecutar
python3 termux_sync.py
```

### Menú CLI
```
1 · Sincronizar saldos
2 · Detectar tokens nuevos
3 · Exportar CSV transacciones
4 · Estado del servidor
5 · Salir
```

### Variables de entorno
```bash
KONRE_API       # Base URL del backend
KONRE_JWT       # Token JWT (24h)
KONRE_AES_KEY   # Clave AES-256 en hex (64 chars)
```

---

## APK Android — Build Script

### Método 1: Script automático
```bash
chmod +x build_apk.sh
bash build_apk.sh
```

### Método 2: PWA instalable
```
URL: https://andobamux-gxzaxdxx.manus.space
Chrome → Menú → "Añadir a pantalla de inicio"
```

### Permisos APK
```xml
INTERNET, USE_BIOMETRIC, CAMERA (QR), NFC, USB_PERMISSION,
VIBRATE, RECEIVE_BOOT_COMPLETED, POST_NOTIFICATIONS
```

### KonreBridge (JS ↔ Java)
```javascript
// Disponible en la WebApp como window.KonreBridge
window.KonreBridge.getDeviceInfo()   // → JSON con modelo y SDK
window.KonreBridge.hasUsb()          // → boolean (USB-OTG detectado)
window.KonreBridge.vibrate(100)      // → feedback háptico
window.KonreBridge.showToast("msg")  // → toast nativo Android
window.KonreBridge.getAppVersion()   // → "21.0.0"
```

---

## Redes Blockchain Soportadas

| Red | Chain ID | RPC | Explorer |
|-----|----------|-----|----------|
| ETH (Ethereum) | 1 | alchemy.com | etherscan.io |
| BSC (BNB Chain) | 56 | binance.org | bscscan.com |
| MATIC (Polygon) | 137 | polygon-rpc.com | polygonscan.com |
| ARB (Arbitrum) | 42161 | arbitrum.io | arbiscan.io |
| OP (Optimism) | 10 | optimism.io | optimistic.etherscan.io |
| BASE | 8453 | base.org | basescan.org |
| TRX (TRON) | — | trongrid.io | tronscan.org |
| SOL (Solana) | — | mainnet-beta.solana.com | solscan.io |

---

## KONRE Autopilot — Motor IA

### Modelo: `claude-sonnet-4-6`
```json
{
  "model": "claude-sonnet-4-6",
  "max_tokens": 1000,
  "system": "Eres KONRE, piloto automático de trading en español MX...",
  "dryRunDefault": true,
  "indicators": ["RSI", "MACD", "BB", "EMA20", "EMA50"],
  "rsiOversold": 30,
  "rsiOverbought": 70,
  "maxRiskPerTrade": "5%"
}
```

### Flujo de decisión
```
Precio en tiempo real (CoinGecko)
    ↓
Cálculo de indicadores (RSI/MACD/BB)
    ↓
KONRE LLM analiza señales
    ↓
Genera SUGERENCIA (no ejecuta automático por defecto)
    ↓
Usuario confirma → Firma USB-OTG → Broadcast
```

---

## Pagos Stripe (Fases 7)

### Métodos disponibles
- 💳 **Tarjeta** (Visa/MC/AMEX)
- 🏪 **OXXO** (efectivo México)
- 🏦 **SPEI** (transferencia bancaria MX)

### Webhook
```
POST https://andobamux-gxzaxdxx.manus.space/api/stripe/webhook
Evento: payment_intent.succeeded → crear notificación
```

---

## Exportación CSV

### Formato propio (transactions_export)
```csv
ID,Balance,BalanceIn,TxID,TxIDIn,Fees,Confirmations,Date,Time,Chain,Coin,Message,NumericPrice,SelectedCurrencyCode,Address,TxType
```

### Formato Zodl (Zodl_Transaction_History)
```csv
Date,Received Quantity,Received Currency,Sent Quantity,Sent Currency,Fee Amount,Fee Currency,Tag
```

---

## SMS Gateway

- `sms-gateway-backup.json` actualmente vacío (`[]`)
- Listo para conectar con **SMS Gateway Android** (app)
- Canal: notificaciones críticas vía SMS al propietario
- Endpoint futuro: `POST /api/notifications/sms`

---

## TODO Pendiente (Fases 3–10)

```
✅ Fase 1: BD + esquema + migraciones
✅ Fase 2: Landing + auth + login
✅ Fase 3: Dashboard cartera Web3
✅ Fase 4: KONRE Autopilot IA (preview)
🔲 Fase 5: Endpoints Termux (backend)
🔲 Fase 6: USB-OTG firma real (eth_account)
✅ Fase 7: UI Stripe MXN
✅ Fase 8: Notificaciones UI
✅ Fase 9: Configuración completa
🔲 Fase 10: Pruebas + despliegue + monitoreo
```

---

## Archivos del Proyecto

```
andobamux/
├── config/
│   └── konre.config.json       ← Configuración central
├── src/
│   ├── andobamux_wallet.jsx    ← Dashboard principal (5 módulos)
│   └── andobamux_modules.jsx   ← Módulos adicionales (4 módulos)
├── termux/
│   ├── termux_sync.py          ← CLI Python Termux
│   └── build_apk.sh            ← Script build APK Android
└── KONRE_DOCS.md               ← Este archivo
```

---

*ANDOBAMUX WALLET_MX · KONRE v21 · 🇲🇽 México · AES-256-GCM · Termux*

