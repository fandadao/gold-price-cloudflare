# gold-price-cloudflare
黄金价格监控系统（人民币/克）

自定义配置说明

环境变量设置：

GOLD_API_KEY: 你的goldapi.io API密钥

在Cloudflare Workers设置中添加KV命名空间并绑定变量名GOLD_PRICE_KV

主要配置参数：


; CONFIG object holds configuration constants for the application.
; 
; CACHE_TTL: The time-to-live for cached data in seconds. Default is 300 seconds (5 minutes).
; OUNCE_TO_GRAM: Conversion factor to convert weight from ounces to grams.
; GOLD_API_URL: The primary API endpoint for fetching gold price in CNY.
; FALLBACK_API_URL: The fallback API endpoint for fetching gold price in case the primary API fails.
; API_KEY: The API key for authentication, retrieved from an environment variable.
; KV_NAMESPACE: The KV namespace binding for storing and retrieving cached gold price data.
<JAVASCRIPT>
const CONFIG = {
  CACHE_TTL: 300, // 5分钟缓存(秒)
  OUNCE_TO_GRAM: 31.1035, // 重量转换系数
  GOLD_API_URL: 'https://www.goldapi.io/api/XAU/CNY', // 主API
  FALLBACK_API_URL: 'https://api.metals.live/v1/spot/gold', // 备用API
  API_KEY: GOLD_API_KEY, // 从环境变量获取
  KV_NAMESPACE: GOLD_PRICE_KV // 绑定的KV命名空间
};
  
数据流说明：

用户访问根路径/时，返回完整的前端HTML页面
前端页面通过/api/gold获取JSON格式的黄金价格数据
Worker首先检查KV缓存，有有效缓存则直接返回
无缓存时从API获取实时数据，处理后存入KV并返回
历史数据生成：

由于大多数免费API不提供历史数据，我们模拟生成
在最新价格基础上添加随机波动生成24小时、7天和30天数据
实际应用中可替换为真实的历史数据API
部署步骤
在Cloudflare Dashboard中：

创建一个新的Worker
添加KV命名空间并绑定到Worker
设置环境变量GOLD_API_KEY
将完整代码粘贴到Worker编辑器中

部署Worker

(可选)设置自定义域名

功能特点
人民币/克计价：直接显示中国人民熟悉的单位和货币
响应式设计：适配各种屏幕尺寸
数据可视化：使用Chart.js展示价格走势
自动刷新：前端每5分钟自动更新数据
缓存机制：使用Cloudflare KV减少API调用
容错处理：主API不可用时使用备用API
完整包含：前后端一体化部署，无需额外基础设施

