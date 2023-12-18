<!-- IndexBar Demo -->
<script>
	import { IndexBar, Button, Toast } from 'stdf';

	const addressList = [
		{ index: 'A', title: 'A', child: [{ text: '澳门' }, { text: '安宁' }, { text: '安庆' }, { text: '鞍山' }] },
		{ index: 'B', title: 'B', child: [{ text: '北京' }, { text: '保定' }, { text: '包头' }, { text: '宝鸡' }] },
		{ index: 'C', title: 'C', child: [{ text: '承德' }, { text: '沧州' }, { text: '赤峰' }] },
		{ index: 'D', title: 'D', child: [{ text: '大同' }, { text: '大连' }, { text: '大理' }] },
		{ index: 'E', title: 'E', child: [{ text: '恩施' }, { text: '鄂尔多斯' }, { text: '峨眉山' }] },
		{ index: 'F', title: 'F', child: [{ text: '抚顺' }, { text: '佛山' }, { text: '福泉' }] },
		{ index: 'G', title: 'G', child: [{ text: '个旧' }, { text: '桂林' }, { text: '赣州' }] },
		{ index: 'H', title: 'H', child: [{ text: '邯郸' }, { text: '呼和浩特' }, { text: '呼伦贝尔' }] },
		{ index: 'J', title: 'J', child: [{ text: '吉林' }, { text: '酒泉' }, { text: '晋江' }] },
		{ index: 'K', title: 'K', child: [{ text: '昆明' }, { text: '昆山' }, { text: '开封' }] },
		{ index: 'L', title: 'L', child: [{ text: '拉萨' }, { text: '兰州' }, { text: '泸州' }] },
		{ index: 'M', title: 'M', child: [{ text: '绵阳' }, { text: '马鞍山' }, { text: '牡丹江' }] },
		{ index: 'N', title: 'N', child: [{ text: '南昌' }, { text: '南京' }, { text: '宁波' }] },
		{ index: 'P', title: 'P', child: [{ text: '莆田' }, { text: '攀枝花' }, { text: '平顶山' }] },
		{ index: 'Q', title: 'Q', child: [{ text: '曲靖' }, { text: '青岛' }, { text: '齐齐哈尔' }] },
		{ index: 'R', title: 'R', child: [{ text: '瑞丽' }, { text: '瑞安' }, { text: '瑞金' }] },
		{ index: 'S', title: 'S', child: [{ text: '三亚' }, { text: '上海' }, { text: '深圳' }] },
		{ index: 'T', title: 'T', child: [{ text: '台州' }, { text: '太原' }, { text: '天津' }] },
		{ index: 'W', title: 'W', child: [{ text: '武汉' }, { text: '无锡' }, { text: '芜湖' }] },
		{ index: 'X', title: 'X', child: [{ text: '西安' }, { text: '厦门' }, { text: '宣威' }] },
		{ index: 'Y', title: 'Y', child: [{ text: '义乌' }, { text: '银川' }, { text: '玉溪' }] },
		{ index: 'Z', title: 'Z', child: [{ text: '昭通' }, { text: '遵义' }, { text: '张家界' }] },
	];
	const emojiList = [
		{ index: '😀', title: '表情与情感', child: [{ text: '😂' }, { text: '🥰' }, { text: '🥱' }, { text: '🧐' }] },
		{ index: '👋', title: '人物与身体', child: [{ text: '👌' }, { text: '🤝' }, { text: '👨‍💻' }, { text: '💪' }] },
		{ index: '🐵', title: '动物与自然', child: [{ text: '🦄' }, { text: '🌱' }, { text: '🦁' }, { text: '🐶' }] },
		{ index: '🍎', title: '食物与饮料', child: [{ text: '🍑' }, { text: '🍞' }, { text: '🍽️' }, { text: '🍩' }] },
		{ index: '🌍', title: '旅行与地理', child: [{ text: '🏔️' }, { text: '🚆' }, { text: '✈️' }, { text: '🛵' }] },
		{ index: '🏆', title: '活动', child: [{ text: '⚽' }, { text: '🎨' }, { text: '🏀' }, { text: '🧨' }] },
		{ index: '📱', title: '物品', child: [{ text: '🎩' }, { text: '💻' }, { text: '🎒' }, { text: '📖' }] },
		{ index: '⚠️', title: '符号标志', child: [{ text: '🚫' }, { text: '⚛️' }, { text: '🛄' }, { text: '🔱' }] },
		{ index: '🏁', title: '旗帜', child: [{ text: '🇨🇳' }, { text: '🇺🇸' }, { text: '🇧🇻' }, { text: '🇦🇹' }] },
	];
	const movieTVList = [
		{
			index: '三',
			title: '三体',
			child: [
				{ text: '地球往事' },
				{ text: '黑暗森林' },
				{ text: '死神永生' },
				{ text: '科学边界' },
				{ text: '射手和农场主' },
				{ text: '寂静的春天' },
				{ text: '宇宙闪烁' },
				{ text: '地球叛军' },
				{ text: '古筝行动' },
				{ text: '智子' },
				{ text: '面壁者' },
				{ text: '咒语' },
				{ text: '黑暗森林' },
				{ text: '危机纪元' },
				{ text: '威慑纪元' },
				{ text: '广播纪元' },
				{ text: '掩体纪元' },
				{ text: '银河纪元' },
				{ text: '时间开始' },
				{ text: '时间之外，我们的宇宙' },
			],
		},
		{
			index: '流',
			title: '流浪地球',
			child: [
				{ text: '人类的勇气和坚毅必将被镌刻在星空之下。' },
				{ text: '希望是像钻石一样珍贵的东西！' },
				{ text: '希望，希望是我们唯一回家的方向。' },
				{ text: '让人类永远保持理智，是一种奢求。' },
				{ text: '道路千万条，安全第一条；行车不规范，亲人两行泪！' },
				{ text: '那时候人们还没注意到太阳，人们更喜欢叫做钱的东西。' },
				{ text: '为了我们的孩子，没有什么不能失去的了' },
				{ text: 'moss没有叛逃!' },
				{ text: '火石没了，我老婆孩子全他妈白死了！' },
			],
		},
		{
			index: '仙',
			title: '仙剑奇侠传',
			child: [
				{ text: '既不回头，何必不忘。既然无缘，何必誓言。' },
				{ text: '没有值得不值得，只有愿意不愿意。' },
				{ text: '不曾得到，谈何放下。既已得到，又如何放下。' },
				{ text: '轮回的是故人，不变的是初心！' },
				{ text: '明夕何夕，君已陌路。' },
				{ text: '我曾经也拥有过爱，只是，我放弃了。' },
				{ text: '你以为每天看到的东西，就会抓得住吗？' },
				{ text: '世有无情人，却有痴情妖。' },
				{ text: '君子，小人，皆在一念思量。' },
				{ text: '到家了，一定要叫醒我，我怕我醒不过来。' },
				{ text: '如果得道就是牺牲所有人来成全自己，我宁愿执迷不悟。' },
				{ text: '我们今日一别，让我们十年后再相见，不见不散！' },
				{ text: '一杯愁绪，几段情缘，莫过，与君对酒当歌。' },
			],
		},
	];
	const navHeight = 48;
	const bottomHeight = 50;
	const height = window.innerHeight - navHeight - bottomHeight;
	/**
	 * @type {'base' | 'full' | 'none'}
	 */
	let radius = 'base';
	let dataFlag = 0;
	let scrollAlign = true;
	let injClassList = [];
	const changeRadiusFun = () => {
		radius = radius === 'base' ? 'full' : radius === 'full' ? 'none' : 'base';
	};
	const changeListFun = () => {
		dataFlag = dataFlag === 0 ? 1 : dataFlag === 1 ? 2 : 0;
	};
	const changeScrollAlignFun = () => {
		scrollAlign = !scrollAlign;
	};
	const changeClassFun = () => {
		injClassList = !injClassList.length ? ['!text-3xl text-center', 'text-xs text-center !py-1'] : [];
	};
	let visible = false;
	let toastObj = { index: 0, group: {}, childIndex: 0, child: {} };
</script>

{#if dataFlag === 0}
	<IndexBar
		data={addressList}
		{radius}
		{height}
		{scrollAlign}
		titleInjClass={injClassList[0]}
		textInjClass={injClassList[1]}
		top={navHeight}
		on:clickchild={e => {
			toastObj = e.detail;
			visible = true;
		}}
	/>
{:else if dataFlag === 1}
	<IndexBar
		data={emojiList}
		{radius}
		{height}
		{scrollAlign}
		titleInjClass={injClassList[0]}
		textInjClass={injClassList[1]}
		top={navHeight}
		on:clickchild={e => {
			toastObj = e.detail;
			visible = true;
		}}
	/>
{:else}
	<IndexBar
		data={movieTVList}
		{radius}
		{height}
		{scrollAlign}
		titleInjClass={injClassList[0]}
		textInjClass={injClassList[1]}
		top={navHeight}
		on:clickchild={e => {
			toastObj = e.detail;
			visible = true;
		}}
	/>
{/if}
<Toast
	bind:visible
	message={`点击了第 ${toastObj.index + 1} 组（${toastObj.group.title}）中的第 ${toastObj.childIndex + 1} 项（${toastObj.child.text}）`}
/>

<div class="sticky flex justify-between bottom-0 px-2 z-10 bg-white/90 dark:bg-black/90">
	<Button fill="lineTheme" size="auto" injClass="text-xs px-2" on:click={changeListFun}>切换数据</Button>
	<Button fill="lineTheme" size="auto" injClass="text-xs px-2" on:click={changeScrollAlignFun}>
		{scrollAlign ? '关闭' : '开启'}滚动对齐
	</Button>
	<Button fill="lineTheme" size="auto" injClass="text-xs px-2" on:click={changeRadiusFun}>切换圆角</Button>
	<Button fill="lineTheme" size="auto" injClass="text-xs px-2" on:click={changeClassFun}>切换样式</Button>
</div>
