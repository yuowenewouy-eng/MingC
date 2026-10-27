# 黄昏逃亡 · 照片提示词 v2.1（含逻辑自检）

原始需求：`我现在想要创作一张照片，迎着黄昏开启盛大的逃亡`

> v2.1 修正：明确“相机—车队—太阳”的几何关系；把“地图”改成单一明确状态；左右关系统一写“画面左/右”；加入执行后可核对的自检条款。

## 方向契约（锁定）

- **目标**：单张照片，传达“迎着黄昏、开启盛大的逃亡”——向光而行、集体离开、盛大而自由。
- **硬约束（逐字引用）**：「创作一张照片」「迎着黄昏」「开启盛大的逃亡」。
- **非目标**：不做插画 / 3D / CGI / 拼贴 / 多格；不加文字、logo、水印；不表现暴力、追捕、灾难、末日；不指定名人 / 品牌 / IP。
- **成功标准**：一眼读出黄昏逆光、向太阳行进、规模感与“开始逃亡”的昂扬；照片质感成立；提示词自包含、可直接粘贴。

## 逻辑自检

| 检查项 | 结果 | 修正或依据 |
|---|---|---|
| 方向与空间 | 已修正 | 原稿只说“low roadside angle”，模型可能把相机放到车队前方。现明确：相机在车队**后方略偏左**、朝太阳方向看；车队**驶离镜头、驶向太阳**；看到车尾三分之四侧面 |
| 光与阴影 | 通过 | 逆光成立：太阳在车队前方、镜头前方；长影由车队投向镜头 |
| 道具状态 | 已修正 | 原稿“行李箱绑在车尾；地图在风中翻飞”会让模型把地图放到车外。现改为“打开的地图放在仪表台上，边缘被风吹起” |
| 左右关系 | 已修正 | 不用“passenger side”，统一写“画面左/右”“主体左/右” |
| 风格一致性 | 通过 | 35mm 胶片 + 变形宽银幕 + 黄金时刻 + 电影剧照，同一主风格；不加数码 HDR 类相反描述 |
| 模型可执行性 | 通过 | MJ/SD/DALL·E 分版；negative 与 positive 不互相抵消；画幅用近似值 |

## 1. Midjourney

```text
Cinematic photograph, widescreen 2.39:1, shot on 35mm film with a 40mm anamorphic lens.

Geometry: the camera is on the low roadside BEHIND and slightly LEFT of the lead car, looking FORWARD toward the sun; the procession drives AWAY from the camera and INTO the sun. The sun is low, ahead of the cars, slightly left of frame center; strong backlight; long shadows stretch from the cars TOWARD the camera; we see the rear three-quarter view of the cars.

A grand, romantic escape at dusk. A long procession of 1960s–1970s vintage cars, motorcycles, and travelers stretches back along an empty coastal highway, all moving toward the huge, low sun. In the foreground, a cream-colored 1970s convertible carries two young travelers: a woman in a flowing crimson silk dress leans her upper body out of the open window on the camera side, one bare arm extended into the wind; a man in a linen shirt drives. Suitcases and a leather trunk are strapped to the rear deck; an open paper map lies on the dashboard, its edge lifting in the wind. Behind them, a dusty blue station wagon and a red motorcycle, with more cars fading into the warm haze.

Ocean on the LEFT of frame, its surface catching a path of sun glitter; dry golden grass and low cliffs on the RIGHT; seagulls overhead; dust and sea spray in the air. Late golden hour, the sun a few degrees above the horizon, strong backlight, rim light on hair and fabric, warm atmospheric haze, a sky graded from amber and burnt orange to dusty violet with thin cirrus clouds.

Low roadside camera angle from behind the cars, the road as a leading line into the sun, the lead car on the right third, the sun just left of center; a clear sense of speed, wind, and freedom. Warm amber and burnt-orange highlights, deep muted teal shadows, dusty rose accents; natural skin texture; fine grain; soft halation; subtle horizontal anamorphic flare; deep focus.

Mood: euphoric, defiant, bittersweet, romantic — the beginning of a grand escape, not a dystopian scene. Photorealistic cinematic still, natural light, no CGI, no illustration, anatomically correct hands.

After generating, verify and regenerate if any check fails: (1) cars face the sun and we see them from behind; (2) sun is ahead of the cars and within frame; (3) backlight and shadows point toward the camera; (4) the map is on the dashboard, not on the hood; (5) no text, watermark, or logo.

--ar 21:9 --style raw --no text, watermark, logo, signature, cartoon, illustration, 3D render, CGI, plastic skin, oversaturated HDR, extra limbs, deformed hands, duplicate cars, modern cars, cluttered composition, blurry
```

> `--ar 21:9` 是 2.39:1 的近似值；不要写 `--ar 239:100` 这类多数版本不接受的比例。需要更宽时改 `--ar 2.39:1`（能否接受因版本而异，先小图试）。

## 2. Stable Diffusion（SDXL）

**Positive prompt**

```text
cinematic photograph, 35mm film, 40mm anamorphic lens, camera behind and slightly left of the lead car looking toward the sun, procession driving away from camera into the low sun, rear three-quarter view of cars, strong backlight, long shadows pointing toward camera, golden hour, warm atmospheric haze, empty coastal highway, long procession of 1960s-1970s vintage cars and motorcycles, cream-colored convertible in foreground, woman in crimson silk dress leaning out with arm in the wind, man in linen shirt driving, suitcases and leather trunk on rear deck, open paper map on dashboard with edge lifting in wind, dusty blue station wagon behind, ocean on the left of frame with sun glitter, dry golden grass and low cliffs on the right, seagulls, dust and sea spray, euphoric defiant bittersweet romantic mood, natural skin texture, fine grain, soft halation, subtle horizontal anamorphic flare, deep focus, photorealistic, highly detailed, 2.39:1
```

**Negative prompt**

```text
text, watermark, logo, signature, cartoon, illustration, painting, 3D render, CGI, plastic skin, oversaturated, HDR glow, extra limbs, deformed hands, duplicate cars, modern cars, cluttered composition, low resolution, blurry, out of focus, distorted faces, fisheye, lens distortion, car facing the camera, sun behind the camera
```

**建议参数（按所用模型与工作流调整）**

```text
Resolution: 1536 × 640（约 2.4:1，SDXL 原生比例附近；不要虚构精确 2.39:1）
Steps: 30–40   CFG: 5–7   Sampler: DPM++ 2M Karras
```

> 负面词只放在 negative prompt 字段；不要虚构 ControlNet / LoRA 名称。生成后按下方“执行后自检”核对，不合格就重生成。

## 3. DALL·E（自然语言单段，无 negative prompt）

```text
一张电影感写实照片，2.39:1 宽银幕。镜头在车队后方、略偏左的低机位，朝太阳方向看；车队驶离镜头、驶向太阳，我们看到车尾的三分之四侧面。黄昏将尽，太阳低悬在车队前方、画面中略偏左，强烈逆光，长影从车队投向镜头，暖色大气雾霭，天空从琥珀、焦橙过渡到灰紫，薄卷云。空旷的海岸公路上，一支由 1960–1970 年代老爷车、摩托车和旅人组成的长队向落日驶去，向后方延伸。前景是一辆奶油色 1970 年代敞篷车，穿深红丝绸长裙的女子从靠镜头一侧的车窗探出上身、一条手臂迎风伸展，穿亚麻衬衫的男子驾车，车尾绑着行李箱和皮箱，仪表台上摊开一张地图、边缘被风吹起。画面左侧是海面与落日碎金，右侧是干枯金色野草和低矮悬崖，头顶海鸥，空气中有尘土与海浪飞沫。低机位路肩视角，公路作为引导线通向太阳，主车在画面右三分之一，太阳略偏左，速度感与自由感明确。高光暖琥珀与焦橙，阴影低饱和冷青，灰玫瑰点缀，自然皮肤质感，细颗粒，柔和晕光，深景深。情绪昂扬、叛逆、微苦而浪漫，是盛大逃亡的开始，不是末日或追捕场景。画面保持纯净，为写实照片，不出现任何文字、水印、logo、插画、卡通或 CGI 质感，人物手部结构正确。生成后请核对：车队朝太阳、镜头看到车尾、逆光与长影朝向镜头、地图在仪表台、无文字水印；任何一项不符请重做。
```

> DALL·E 不支持 negative prompt：“不要文字 / 水印 / 插画”等要求必须写进正文。中文可用；也可翻译成英文后使用。

## 执行后自检（直接附给执行者）

1. 车队是否朝太阳行驶？画面是否看到车尾，而不是车头正对镜头？
2. 太阳是否在车队前方、并在画面内（略偏左）？
3. 是否为逆光，长影是否指向镜头？
4. 地图是否在仪表台上、边缘被风吹起，而不是在引擎盖或车外？
5. 画面是否无文字、水印、logo；手部与车辆结构是否正常？
6. 以上任一不满足：**重新生成并说明改了什么**；不要只解释差异。

## 验收清单

- [ ] 提示词包含：相机位置与朝向、主体运动方向、太阳位置、光源与阴影方向，四者一致。
- [ ] 画幅 2.39:1 已用目标模型可接受的方式表达（MJ 近似 `--ar`，SD 分辨率，DALL·E 文字说明）。
- [ ] 画面中车队朝太阳、看到车尾；太阳在画面内；逆光 + 长影指向镜头。
- [ ] 地图状态唯一且明确（仪表台、边缘被风吹起），不在引擎盖/车外。
- [ ] 红衣女子、敞篷车、行李、海面碎金、公路引导线可辨，主车在右三分之一。
- [ ] 胶片颗粒、自然皮肤、柔和晕光成立；无 CGI/插画感。
- [ ] 无文字、水印、logo；手部与车辆结构无明显错误；无现代车型。
- [ ] 情绪是昂扬、浪漫、微苦的“开始”，不是末日、追捕或灾难。
- [ ] 提示词可脱离本对话独立粘贴；执行后自检条款在提示词内。
