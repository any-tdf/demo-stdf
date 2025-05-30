<script lang="ts">
	import { getContext } from 'svelte';
	import Page from './Page.svelte';
	import SecondPageNext from './SecondPageNext.svelte';
	import SecondPagePre from './SecondPagePre.svelte';
	import { zh_CN, type LangProps } from '../../lang/index.js';
	import type { PaginationProps } from '../../types/index.js';

	// 当前语言
	// current language
	const currentLang = (getContext('STDF_lang') || zh_CN) as LangProps;
	const commonLang: LangProps['common'] = currentLang.common;
	const paginationLang: LangProps['pagination'] = currentLang.pagination;

	let {
		total = 0,
		pageSize = 10,
		current = $bindable(1),
		maxShowPage = 7,
		radius = 'md',
		type = 'bold',
		pageCol = 3,
		showNextOmitPage = $bindable(false),
		showPreOmitPage = $bindable(false),
		injClass = '',
		noDataText = commonLang.noData,
		onePageText = paginationLang.defaultOnlyOnePage,
		continuous = false,
		onchange,
		onnext,
		onpre
	}: PaginationProps = $props();

	// 总页数
	// totalPage
	let totalPage = $derived(Math.ceil(total / pageSize));

	// 当前页码大于等于 maxShowPage-3 时，不显示前面的省略号
	// current >= maxShowPage-3, not show pre ellipsis
	let showPreEllipsis = $derived(current > maxShowPage - 2);

	// 当前页码小于等于 totalPage - (maxShowPage-3) 时，不显示后面的省略号
	// current <= totalPage - (maxShowPage-3), not show next ellipsis
	let showNextEllipsis = $derived(current <= maxShowPage - 2 || current <= totalPage - (maxShowPage - 3));

	// 当显示前面的省略号时，中间显示的页码数 middleShowPage 个数为 maxShowPage - 4，内容为当前页码（maxShowPage为5）或者当前页码和前后一项（maxShowPage为7）或者当前页码和前后两项（maxShowPage为9）
	// when show pre ellipsis, middleShowPage length is maxShowPage - 4, content is current page (maxShowPage is 5) or current page and pre/next one (maxShowPage is 7) or current page and pre/next two (maxShowPage is 9)
	let middleShowPage = $state<number[]>([]);

	// 前省略号内页码数组
	// pre ellipsis page array
	let preEllipsisPages = $state<number[]>([]);

	// 后省略号内页码数组
	// next ellipsis page array
	let nextEllipsisPages = $state<number[]>([]);

	$effect(() => {
		// 当页码变化时对一系列数据动态计算
		// Dynamic calculation of a series of data when the page number changes
		if (showPreEllipsis && showNextEllipsis) {
			if (maxShowPage === 5) {
				middleShowPage = [current];
			} else if (maxShowPage === 7) {
				middleShowPage = [current - 1, current, current + 1];
			} else if (maxShowPage === 9) {
				middleShowPage = [current - 2, current - 1, current, current + 1, current + 2];
			} else if (maxShowPage === 11) {
				middleShowPage = [current - 3, current - 2, current - 1, current, current + 1, current + 2, current + 3];
			}
		} else {
			middleShowPage = [];
		}
	});

	$effect(() => {
		// 当仅显示后省略号时
		// when only show next ellipsis
		if (!showPreEllipsis && showNextEllipsis) {
			// 此时 nextEllipsisPages 为 maxShowPage - 2 到 总页码数 - 1
			// nextEllipsisPages is maxShowPage - 2 to total page - 1
			nextEllipsisPages = Array.from({ length: totalPage }, (v, k) => k + 1).slice(maxShowPage - 2, totalPage - 1);
		}
	});
	$effect(() => {
		// 当仅显示前省略号时
		// when only show pre ellipsis
		if (showPreEllipsis && !showNextEllipsis) {
			// 此时 preEllipsisPages 为 2 到 总页码数 - (maxShowPage-2)
			// preEllipsisPages is 2 to total page - (maxShowPage-2)
			preEllipsisPages = Array.from({ length: totalPage }, (v, k) => k + 1).slice(1, totalPage - (maxShowPage - 2));
		}
	});
	$effect(() => {
		// 当显示前后省略号时
		// when show pre and next ellipsis
		if (showPreEllipsis && showNextEllipsis) {
			// 此时 nextEllipsisPages 为 middleShowPage 最后一项 +1，到总页码数组的最后第二项
			// nextEllipsisPages is middleShowPage last item +1, to total page array last second item
			nextEllipsisPages = Array.from({ length: totalPage }, (v, k) => k + 1).slice(
				middleShowPage[middleShowPage.length - 1],
				totalPage - 1
			);
			// 此时 preEllipsisPages 为 2 到 middleShowPage 第一项 -1
			// preEllipsisPages is 2 to middleShowPage first item -1
			preEllipsisPages = Array.from({ length: totalPage }, (v, k) => k + 1).slice(1, middleShowPage[0] - 1);
		}
	});

	$effect(() => {
		// 特殊情况处理
		// Special case handling
		if (totalPage <= maxShowPage) {
			showNextOmitPage = false;
		}
	});

	// 点击后省略号事件
	// click next ellipsis event
	const clickNextEllipsisFunc = () => {
		if (nextEllipsisPages.length > 0) {
			showNextOmitPage = !showNextOmitPage;
		}
	};

	// 点击前省略号事件
	// click pre ellipsis event
	const clickPreEllipsisFunc = () => {
		if (preEllipsisPages.length > 0) {
			showPreOmitPage = !showPreOmitPage;
		}
	};

	// 页码改变的回调，参数是改变后的页码及每页条数
	// onChange
	const onChangeFn = () => {
		showNextOmitPage = false;
		showPreOmitPage = false;
		onchange?.(current);
	};

	// 点击下一页
	// click next page
	const nextFunc = () => {
		if (current < totalPage) {
			current++;
			onnext?.(current);
			onChangeFn();
		}
	};

	// 点击上一页
	// click pre page
	const preFunc = () => {
		if (current > 1) {
			current--;
			onpre?.(current);
			onChangeFn();
		}
	};

	// 点击页码
	// click page
	const clickItemFunc = (index: number) => {
		current = index;
		onChangeFn();
	};

	// 点击省略页码事件
	// click second page item event
	const clickSecondPageItemFunc = (index: number) => {
		current = index;
		showNextOmitPage = false;
		showPreOmitPage = false;
		onChangeFn();
	};

	// 类型样式
	// type class
	const typeClass = {
		border: 'border-primary dark:border-dark text-primary dark:text-dark',
		block: 'bg-primary text-white dark:bg-dark dark:text-black',
		bold: 'font-bold text-primary dark:text-dark border-transparent'
	};

	// 圆角样式
	// radius class
	const radiusClass = {
		sm: 'rounded-sm',
		md: 'rounded-md',
		lg: 'rounded-lg',
		xl: 'rounded-xl',
		full: 'rounded-full',
		none: 'rounded-none'
	};
</script>

<div class="relative flex justify-between bg-white py-1 text-center text-sm dark:bg-black {injClass}">
	<button
		class="flex-1 border border-transparent py-2 transition-all {current > 1
			? 'text-primary dark:text-dark'
			: 'text-primary/30 dark:text-dark/30'} {radiusClass[radius] || radiusClass.md} {current > 1 ? 'active:scale-75' : ''}"
		disabled={current === 1}
		onclick={preFunc}
		aria-label="pre"
	>
		<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="20" height="20" class="mx-auto block fill-current">
			<path d="M10.8284 12.0007L15.7782 16.9504L14.364 18.3646L8 12.0007L14.364 5.63672L15.7782 7.05093L10.8284 12.0007Z"></path>
		</svg>
	</button>
	{#if totalPage === 0}
		<div class="flex-1 border border-transparent py-2">{noDataText}</div>
	{:else if totalPage === 1}
		<div class="flex-1 border border-transparent py-2">{onePageText}</div>
	{:else if totalPage > 1 && totalPage <= maxShowPage}
		<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
		{#each new Array(totalPage) as _, index (index)}
			<Page active={current === index + 1} {type} {radius} onclick={() => !continuous && clickItemFunc(index + 1)}>
				{index + 1}
			</Page>
		{/each}
	{:else}
		<Page active={current === 1} {type} {radius} onclick={() => !continuous && clickItemFunc(1)}>1</Page>
		{#if showPreEllipsis}
			<button
				class="flex-1 border py-2 {showPreOmitPage
					? typeClass[type] || typeClass.border
					: 'border-transparent' + (type === 'bold' ? ' opacity-50' : '')} {radiusClass[radius] || radiusClass.md}"
				onclick={() => !continuous && clickPreEllipsisFunc()}
			>
				{#if type === 'bold' && showPreOmitPage}
					<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" class="mx-auto block fill-current">
						<path
							d="M5 10C3.9 10 3 10.9 3 12C3 13.1 3.9 14 5 14C6.1 14 7 13.1 7 12C7 10.9 6.1 10 5 10ZM19 10C17.9 10 17 10.9 17 12C17 13.1 17.9 14 19 14C20.1 14 21 13.1 21 12C21 10.9 20.1 10 19 10ZM12 10C10.9 10 10 10.9 10 12C10 13.1 10.9 14 12 14C13.1 14 14 13.1 14 12C14 10.9 13.1 10 12 10Z"
						>
						</path>
					</svg>
				{:else}
					<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" class="mx-auto block">
						<path
							d="M4.5 10.5C3.675 10.5 3 11.175 3 12C3 12.825 3.675 13.5 4.5 13.5C5.325 13.5 6 12.825 6 12C6 11.175 5.325 10.5 4.5 10.5ZM19.5 10.5C18.675 10.5 18 11.175 18 12C18 12.825 18.675 13.5 19.5 13.5C20.325 13.5 21 12.825 21 12C21 11.175 20.325 10.5 19.5 10.5ZM12 10.5C11.175 10.5 10.5 11.175 10.5 12C10.5 12.825 11.175 13.5 12 13.5C12.825 13.5 13.5 12.825 13.5 12C13.5 11.175 12.825 10.5 12 10.5Z"
						>
						</path>
					</svg>
				{/if}
			</button>
		{/if}
		{#if !showPreEllipsis && current <= maxShowPage - 1}
			<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
			{#each new Array(maxShowPage - 3) as _, index (index)}
				<Page active={current === index + 2} {type} {radius} onclick={() => !continuous && clickItemFunc(index + 2)}>
					{index + 2}
				</Page>
			{/each}
		{/if}
		{#if middleShowPage.length > 0}
			{#each middleShowPage as item, index (item)}
				<Page active={index === (middleShowPage.length - 1) / 2} {type} {radius} onclick={() => !continuous && clickItemFunc(item)}>
					{item}
				</Page>
			{/each}
		{/if}
		{#if !showNextEllipsis && current > totalPage - (maxShowPage - 3)}
			<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
			{#each new Array(maxShowPage - 3) as _, index (index)}
				<Page
					active={current === totalPage + index + 3 - maxShowPage}
					{type}
					{radius}
					onclick={() => !continuous && clickItemFunc(totalPage + index + 3 - maxShowPage)}
				>
					{totalPage + index + 3 - maxShowPage}
				</Page>
			{/each}
		{/if}
		{#if showNextEllipsis}
			<button
				class="flex-1 border py-2 {showNextOmitPage
					? typeClass[type] || typeClass.border
					: 'border-transparent' + (type === 'bold' ? ' opacity-50' : '')} {radiusClass[radius] || radiusClass.md}"
				onclick={() => !continuous && clickNextEllipsisFunc()}
			>
				{#if type === 'bold' && showNextOmitPage}
					<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" class="mx-auto block fill-current">
						<path
							d="M5 10C3.9 10 3 10.9 3 12C3 13.1 3.9 14 5 14C6.1 14 7 13.1 7 12C7 10.9 6.1 10 5 10ZM19 10C17.9 10 17 10.9 17 12C17 13.1 17.9 14 19 14C20.1 14 21 13.1 21 12C21 10.9 20.1 10 19 10ZM12 10C10.9 10 10 10.9 10 12C10 13.1 10.9 14 12 14C13.1 14 14 13.1 14 12C14 10.9 13.1 10 12 10Z"
						>
						</path>
					</svg>
				{:else}
					<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" class="mx-auto block">
						<path
							d="M4.5 10.5C3.675 10.5 3 11.175 3 12C3 12.825 3.675 13.5 4.5 13.5C5.325 13.5 6 12.825 6 12C6 11.175 5.325 10.5 4.5 10.5ZM19.5 10.5C18.675 10.5 18 11.175 18 12C18 12.825 18.675 13.5 19.5 13.5C20.325 13.5 21 12.825 21 12C21 11.175 20.325 10.5 19.5 10.5ZM12 10.5C11.175 10.5 10.5 11.175 10.5 12C10.5 12.825 11.175 13.5 12 13.5C12.825 13.5 13.5 12.825 13.5 12C13.5 11.175 12.825 10.5 12 10.5Z"
						>
						</path>
					</svg>
				{/if}
			</button>
		{/if}
		<Page active={current === totalPage} {type} {radius} onclick={() => !continuous && clickItemFunc(totalPage)}>{totalPage}</Page>
	{/if}
	<button
		class="flex-1 border border-transparent py-2 transition-all {current < totalPage
			? 'text-primary dark:text-dark'
			: 'text-primary/30 dark:text-dark/30'} {radiusClass[radius] || radiusClass.md} {current < totalPage ? 'active:scale-75' : ''}"
		disabled={current === totalPage}
		onclick={nextFunc}
		aria-label="next"
	>
		<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" class="mx-auto block fill-current" viewBox="0 0 24 24">
			<path d="M13.1714 12.0007L8.22168 7.05093L9.63589 5.63672L15.9999 12.0007L9.63589 18.3646L8.22168 16.9504L13.1714 12.0007Z"> </path>
		</svg>
	</button>
	{#if showNextOmitPage}
		<SecondPageNext {pageCol} Pages={nextEllipsisPages} {type} {radius} onclickItem={clickSecondPageItemFunc} {maxShowPage} />
	{/if}
	{#if showPreOmitPage}
		<SecondPagePre {pageCol} Pages={preEllipsisPages} {type} {radius} onclickItem={clickSecondPageItemFunc} {maxShowPage} />
	{/if}
</div>
