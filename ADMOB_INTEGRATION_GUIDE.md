# Guia de Integração AdMob - Tile Match Game

## ✅ Estrutura Implementada

### 1. SDK Inicializado
- ✅ Google AdSense SDK carregado no `<head>`
- ✅ AdManager inicializado automaticamente ao carregar página
- ✅ Sistema de pré-carregamento de anúncios

### 2. Banner Ads Implementados
**Localização:** Parte inferior da tela, acima dos botões de skills

**Código HTML:**
```html
<div class="banner-ad-container">
    <ins class="adsbygoogle"
         style="display:inline-block;width:320px;height:50px"
         data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
         data-ad-slot="XXXXXXXXXX"></ins>
</div>
```

**Status:** ⚠️ Requer IDs reais do AdMob

### 3. Anúncios Intersticiais (Tela Cheia)
**Implementação:** Classe `AdManager` com métodos:
- `init()` - Inicializa sistema
- `loadBannerAd()` - Carrega banner
- `preloadInterstitial()` - Pré-carrega intersticial
- `showInterstitial(callback)` - Exibe intersticial
- `simulateAd(callback)` - Simula anúncio (teste)

**Pontos de Exibição:**
- ✅ Game Over → Botão "Continuar (Anúncio)"

---

## 🔧 Configuração Necessária

### Passo 1: Criar Conta Google AdMob
1. Acesse: https://admob.google.com
2. Crie uma conta (se não tiver)
3. Adicione seu aplicativo

### Passo 2: Obter IDs de Unidade de Anúncio

#### Para Testes (Use enquanto desenvolve):
```javascript
// Banner de Teste
data-ad-client="ca-app-pub-3940256099942544"
data-ad-slot="6300978111"

// Intersticial de Teste
// ID: ca-app-pub-3940256099942544/1033173712
```

#### Para Produção:
1. No painel do AdMob, clique em "Unidades de anúncio"
2. Crie unidade de anúncio tipo "Banner" (320x50)
3. Crie unidade de anúncio tipo "Intersticial"
4. Copie os IDs gerados

### Passo 3: Substituir IDs no Código

**No arquivo `tile_match_web.html`:**

1. **Linha ~7 (SDK):**
```html
<!-- Trocar XXXXXXXXXXXXXXXX pelo seu ID do AdMob -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX"
     crossorigin="anonymous"></script>
```

2. **Linha ~452 (Banner):**
```html
<ins class="adsbygoogle"
     style="display:inline-block;width:320px;height:50px"
     data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"  <!-- Seu ID aqui -->
     data-ad-slot="XXXXXXXXXX"></ins>           <!-- ID da unidade Banner -->
```

---

## 📱 Convertendo para Android/iOS (Opcional)

### Opção A: Capacitor (Recomendado)
```bash
npm install -g @capacitor/cli
npx cap init
npx cap add android
npx cap add ios
```

**Adicionar plugin AdMob:**
```bash
npm install @capacitor-community/admob
npx cap sync
```

**Código JavaScript para Mobile:**
```javascript
import { AdMob } from '@capacitor-community/admob';

// Inicializar
await AdMob.initialize({
  requestTrackingAuthorization: true,
});

// Banner
await AdMob.showBanner({
  adId: 'ca-app-pub-XXXXX/YYYYY',
  position: 'BOTTOM_CENTER',
});

// Intersticial
await AdMob.prepareInterstitial({
  adId: 'ca-app-pub-XXXXX/ZZZZZ',
});
await AdMob.showInterstitial();
```

### Opção B: Cordova
```bash
cordova plugin add cordova-plugin-admob-free
```

---

## 🧪 Testando

### Modo de Teste (Ambiente de Desenvolvimento):
1. Use IDs de teste do Google
2. Anúncios aparecerão marcados como "Test Ad"
3. Não gera receita

### Modo de Produção:
1. Substitua por IDs reais
2. ⚠️ **NUNCA clique nos próprios anúncios!**
3. Google pode banir sua conta por cliques fraudulentos
4. Use dispositivos reais para testar

---

## 💡 Dicas Importantes

### Frequência de Anúncios
- ✅ **Atual:** 1 anúncio intersticial por Game Over
- ⚠️ Não abuse! Muito anúncio = usuário desinstala
- 💡 Ideal: 1 anúncio a cada 2-3 Game Overs

### Otimização de Receita
```javascript
// Exemplo: Mostrar anúncio em 50% dos Game Overs
function watchAdAndContinue() {
    if (Math.random() < 0.5) {
        AdManager.showInterstitial(() => restoreGameState());
    } else {
        restoreGameState(); // Sem anúncio
    }
}
```

### Política do Google AdMob
- ❌ Não ocultar área de anúncios
- ❌ Não forçar cliques
- ❌ Não colocar botões muito perto dos anúncios
- ✅ Respeitar área de segurança (50px de margem)
- ✅ Avisar usuário antes de mostrar anúncio

---

## 🐛 Resolução de Problemas

### Anúncios não aparecem:
1. Verificar se IDs estão corretos
2. Usar IDs de teste primeiro
3. Verificar console do navegador (F12)
4. Aguardar 24-48h após criar conta AdMob

### Banner cortado:
- Ajustar `.banner-ad-container` no CSS
- Testar diferentes tamanhos (320x50, 320x100)

### Intersticial não carrega:
```javascript
// Adicionar timeout
setTimeout(() => {
    if (!AdManager.interstitialLoaded) {
        console.log('Timeout: usando simulação');
        AdManager.simulateAd(callback);
    }
}, 5000);
```

---

## 📊 Métricas Recomendadas

Adicionar tracking de eventos:
```javascript
// Exemplo com Google Analytics
function trackAdView(adType) {
    gtag('event', 'ad_impression', {
        'event_category': 'ads',
        'event_label': adType
    });
}

// Usar ao mostrar anúncio
AdManager.showInterstitial(() => {
    trackAdView('interstitial_game_over');
    restoreGameState();
});
```

---

## 🚀 Próximos Passos

1. ✅ Código base implementado
2. ⏳ Obter IDs do AdMob
3. ⏳ Substituir IDs no código
4. ⏳ Testar com IDs de teste
5. ⏳ Publicar e monitorar receita

**Boa sorte com a monetização! 💰**
