<!-- Stepper Demo -->
<script lang="ts">
	import { Stepper, Toast } from '$lib/index.js';

	let asyncValue1 = $state(1);
	let loading1 = $state(false);
	let asyncValue2 = $state(1);
	let loading2 = $state(false);
	let asyncValue3 = $state(1);
	let loading3 = $state(false);
	const handleChange1 = (type: 'increase' | 'decrease') => {
		loading1 = true;
		setTimeout(() => {
			asyncValue1 += type === 'increase' ? 1 : -1;
			loading1 = false;
		}, 2000);
	};
	const handleChange2 = (type: 'increase' | 'decrease') => {
		loading2 = true;
		setTimeout(() => {
			asyncValue2 += type === 'increase' ? 1 : -1;
			loading2 = false;
		}, 3000);
	};
	const handleChange3 = (type: 'increase' | 'decrease') => {
		loading3 = true;
		setTimeout(() => {
			asyncValue3 += type === 'increase' ? 1 : -1;
			loading3 = false;
		}, 4000);
	};
</script>

<div class="mx-4 mt-8 text-lg font-bold">基础用法</div>
<div class="px-4 py-4">
	<Stepper />
</div>

<div class="mx-4 mt-8 text-lg font-bold">设置最大值/最小值/初始值</div>
<div class="px-4 py-4">
	<Stepper min={2} max={10} value={5} />
</div>

<div class="mx-4 mt-8 text-lg font-bold">设置步长</div>
<div class="px-4 py-4">
	<Stepper step={5} />
</div>

<div class="mx-4 mt-8 text-lg font-bold">强调数字区域</div>
<div class="px-4 py-4">
	<Stepper numberHighlight />
</div>

<div class="mx-4 mt-8 text-lg font-bold">强调区域不用主题色</div>
<div class="flex gap-2 px-4 py-4">
	<Stepper theme={false} />
	<Stepper theme={false} numberHighlight />
</div>

<div class="mx-4 mt-8 text-lg font-bold">不同圆角</div>
<div class="flex flex-wrap gap-2 px-4 py-4">
	<div><Stepper radius="none" /></div>
	<div><Stepper radius="sm" /></div>
	<div><Stepper radius="xl" /></div>
	<div><Stepper radius="full" /></div>
	<div><Stepper radius="full" numberHighlight /></div>
	<div><Stepper radius="full" theme={false} /></div>
</div>

<div class="mx-4 mt-8 text-lg font-bold">竖向</div>
<div class="flex justify-around px-2 py-4">
	<Stepper vertical />
	<Stepper vertical numberHighlight />
	<Stepper vertical theme={false} />
	<Stepper vertical radius="none" />
	<Stepper radius="xl" vertical />
</div>

<div class="mx-4 mt-8 text-lg font-bold">固定宽度</div>
<div class="space-x-8 px-2 py-4">
	<Stepper width={160} />
	<Stepper width={80} vertical />
</div>

<div class="mx-4 mt-8 text-lg font-bold">格式化显示数字</div>
<div class="px-4 py-4">
	<div class="mb-2 text-sm">保留一位小数</div>
	<Stepper decimal={1} step={0.1} max={1} min={0.1} value={0.5} />
</div>
<div class="px-4 py-4">
	<div class="mb-2 text-sm">保留四位小数</div>
	<Stepper decimal={4} step={0.0001} max={1} min={0.0001} value={0.5} />
</div>

<div class="mx-4 mt-8 text-lg font-bold">异步显示数字</div>
<div class="px-4 py-4">
	<div class="mb-2 text-sm">Toast 显示异步状态</div>
	<Stepper async={loading1} value={asyncValue1} onincrease={() => handleChange1('increase')} ondecrease={() => handleChange1('decrease')} />
</div>
<Toast bind:visible={loading1} type="loading" message="保存中..." />
<div class="mb-2 px-4 text-sm">内部显示异步状态</div>
<div class="flex gap-2 px-4 py-4">
	<Stepper
		async={loading2}
		value={asyncValue2}
		asyncLoading
		onincrease={() => handleChange2('increase')}
		ondecrease={() => handleChange2('decrease')}
	/>
	<Stepper
		async={loading3}
		value={asyncValue3}
		asyncLoading
		loading={{ type: '1_51' }}
		onincrease={() => handleChange3('increase')}
		ondecrease={() => handleChange3('decrease')}
	/>
</div>

<div class="mx-4 mt-8 text-lg font-bold">外部无 padding</div>
<div class="px-4 py-2">
	<Stepper padding={false} />
</div>
<div class="px-4 py-2">
	<Stepper padding={false} numberHighlight />
</div>
<div class="px-4 py-2">
	<Stepper padding={false} theme={false} />
</div>

<div class="mx-4 mt-8 text-lg font-bold">不同位置注入 Class</div>
<div class="px-4 py-2">
	<Stepper theme={false} injClassOut="bg-gradient-to-r from-[#CE9FFC] to-[#7367F0]" />
</div>
<div class="px-4 py-2">
	<Stepper injClassNum="text-primary dark:text-dark" />
</div>
<div class="px-4 py-2">
	<Stepper injClassBtn="rounded-full" />
</div>
