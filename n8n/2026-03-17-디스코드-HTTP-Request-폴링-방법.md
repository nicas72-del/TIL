### Context
n8n 환경에서 Discord의 'discord-trigger-node' 설치가 불가능한 사용자들이 많아, [HTTP Request + Discord API Polling] 조합으로 워크플로우를 새로 구성하려고 했습니다. 하지만, n8n 웹훅을 Discord의 '인터랙션 엔드포인트 URL'로 설정하려 했으나, Discord는 'https'만 허용하여 문제가 발생했습니다. 

### Core
결국 매 1분마다 폴링하여 Discord API로부터 메시지를 가져오는 방식으로 구현을 시도했습니다. 
```javascript
{
 setInterval(async () => {
   const response = await fetch('https://discord.com/api/messages', {
     method: 'GET',
     headers: { 'Authorization': 'Bot YOUR_BOT_TOKEN' }
   });
   const messages = await response.json();
   // 메시지 ID 체크 로직
 }, 60000);
}
```

### Insight
Discord의 HTTPS 요구 사항 때문에 n8n 웹훅을 직접 사용할 수 없어 폴링 방식을 사용했지만, 이는 효율성 측면에서 바람직하지 못했습니다. 계속해서 DB에 저장 및 유효성 검사를 추가적으로 수행해야 하는 문제가 있어, 소요 시간이 증가하고 워크플로우가 복잡해졌습니다. 

**출처:** [Setup Webhooks on Discord](https://discord.com/developers/docs/resources/webhook)