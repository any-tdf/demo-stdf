<script lang="ts">
	import { getContext } from 'svelte';
	import Popup from '../popup/Popup.svelte';
	import { zh_CN, type LangProps } from '../../lang/index.js';
	import type { NumKeyboardProps } from '../../types/index.js';

	// 当前语言
	// current language
	const currentLang = (getContext('STDF_lang') || zh_CN) as LangProps;
	const commonLang: LangProps['common'] = currentLang.common;

	let {
		type = 'button',
		value = $bindable(''),
		visible = $bindable(true),
		height = '12',
		space = '2',
		p = '2',
		reverse = false,
		done = true,
		dot = true,
		close = false,
		doneText = commonLang.done,
		doneDisabled = $bindable(false),
		radius = 'sm',
		clear = false,
		panelClass = '',
		keyClass = '',
		doneClass = '',
		popup = {},
		onclick,
		onopen,
		onclose
	}: NumKeyboardProps = $props();

	// 高度 class
	// Height class
	const heightClass = { '8': 'h-8', '10': 'h-10', '12': 'h-12', '14': 'h-14', '16': 'h-16', '20': 'h-20' };

	// 根据高度动态数字大小
	// Dynamic number size based on height
	const fontSizeClass = { '8': 'text-sm', '10': 'text-base', '12': 'text-xl', '14': 'text-2xl', '16': 'text-2xl', '20': 'text-2xl' };

	// 间距 class
	// spacing class
	const gapClass = { '0': 'gap-0', '1': 'gap-1', '2': 'gap-2', '3': 'gap-3', '4': 'gap-4' };

	// 内边距 class
	// padding class
	const pClass = { '0': 'p-0', '1': 'p-1', '2': 'p-2', '3': 'p-3', '4': 'p-4' };

	// 圆角 class
	// radius class
	const radiusClass = {
		none: 'rounded-none',
		sm: 'rounded-sm',
		md: 'rounded-md',
		lg: 'rounded-lg',
		xl: 'rounded-xl',
		'2xl': 'rounded-2xl',
		'3xl': 'rounded-3xl',
		full: 'rounded-full'
	};

	// 根据 p、gap、height 计算出键盘高度
	// Calculate the keyboard height based on p, gap, height
	const keyboardHeight = () => {
		const pNum = parseInt(p);
		const gapNum = type === 'button' ? parseInt(space) : 0;
		const heightNum = parseInt(height);
		return (pNum * 2 + gapNum * 3 + heightNum * 4) * 4 + (type === 'block' ? 4 : 0);
	};

	// 按钮式 class
	// Button type class
	const buttonClass = `flex flex-col justify-center items-center shadow-xs font-bold active:scale-95 transition-all duration-100 ${
		heightClass[height] || 'h-12'
	} ${fontSizeClass[height] || 'text-base'} ${radiusClass[radius] || 'rounded-sm'}${keyClass ? ' ' + keyClass : ''}`;

	// 块式 class
	// Block type class
	const blockClass = `flex flex-col justify-center items-center font-bold active:opacity-40 transition-all duration-100 ${
		heightClass[height] || 'h-12'
	} ${fontSizeClass[height] || 'text-base'}${keyClass ? ' ' + keyClass : ''}`;

	// 根据 type 类型，生成不同的基础样式
	// Generate different basic styles based on type type
	const baseClassFunc = (key: string | number) => {
		if (type === 'button') {
			return key === 'done' ? buttonClass : 'bg-white dark:bg-gray-700 ' + buttonClass;
		} else {
			return key === 'done' ? blockClass : 'bg-white dark:bg-gray-700 ' + blockClass;
		}
	};

	// 处理 0 占据的格数
	// Handle the number of grids occupied by 0
	const zeroColFunc = () => {
		// 不显示完成，右下角显示删除按钮，0 最多占 2 格
		// If you don't show it, the delete button will be displayed in the lower right corner, and 0 will occupy up to 2 grids
		if (!done) {
			return dot || close ? 'col-span-1' : 'col-span-2';
		} else {
			return dot && close ? 'col-span-1' : dot || close ? 'col-span-2' : 'col-span-3';
		}
	};

	// 点击按键事件
	// Click the button event
	const clickFunc = (key: string) => {
		// 当 key 不是 close 或 done 时，拼接字符串，是删除时，删除最后一个字符
		// When key is not close or done, splice the string, delete the last character when it is deleted
		if (key !== 'close' && key !== 'done') {
			if (key === 'delete') {
				value = value.slice(0, value.length - 1);
			} else {
				value += key;
			}
		}
		// close 和 done 事件都会关闭
		// Both close and done events will be closed
		if (key === 'close' || (key === 'done' && !doneDisabled)) {
			visible = false;
		}
		// 派发事件，传递出两个参数，输入的数字字符串和本次点击的类型
		// Dispatch events, pass out two parameters, the input number string and the type of this click
		onclick?.(key);
	};

	// 激活与关闭键盘事件
	// Activate and close the keyboard event
	$effect(() => {
		if (visible) {
			if (clear) {
				value = '';
			}
			onopen?.(keyboardHeight());
		} else {
			onclose?.();
		}
	});
</script>

<Popup bind:visible size={0} mask={{ opacity: '0' }} transitionDistance={keyboardHeight()} {...popup}>
	<div
		class="bg-gray-100 text-center dark:bg-gray-950 {type === 'block' ? 'border-t border-gray-100 dark:border-gray-950' : ''} {pClass[p] ||
			'p-2'}{panelClass ? ' ' + panelClass : ''}"
	>
		<div class="grid {type === 'button' ? gapClass[space] || 'gap-2' : 'gap-px'} {done ? 'grid-cols-4' : 'grid-cols-3'}">
			{#each reverse ? ['7', '8', '9'] : ['1', '2', '3'] as item (item)}
				<button class={baseClassFunc(item)} onclick={() => clickFunc(item)}>{item} </button>
			{/each}
			{#if done}
				<button class={baseClassFunc('delete')} onclick={() => clickFunc('delete')} aria-label="delete">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width={Number(height) * 2}
						height={Number(height) * 2}
						viewBox="0 0 24 24"
						class="mx-auto block fill-current"
					>
						<path
							d="M6.53451 3H20.9993C21.5516 3 21.9993 3.44772 21.9993 4V20C21.9993 20.5523 21.5516 21 20.9993 21H6.53451C6.20015 21 5.88792 20.8329 5.70246 20.5547L0.369122 12.5547C0.145189 12.2188 0.145189 11.7812 0.369122 11.4453L5.70246 3.4453C5.88792 3.1671 6.20015 3 6.53451 3ZM7.06969 5L2.40302 12L7.06969 19H19.9993V5H7.06969ZM12.9993 10.5858L15.8277 7.75736L17.242 9.17157L14.4135 12L17.242 14.8284L15.8277 16.2426L12.9993 13.4142L10.1709 16.2426L8.75668 14.8284L11.5851 12L8.75668 9.17157L10.1709 7.75736L12.9993 10.5858Z"
						>
						</path>
					</svg>
				</button>
			{/if}
			{#each ['4', '5', '6'] as item (item)}
				<button class={baseClassFunc(item)} onclick={() => clickFunc(item)}>{item} </button>
			{/each}
			{#if done}
				<button
					class="{baseClassFunc('done')} bg-primary dark:bg-dark row-span-3 h-full text-white dark:text-black {doneDisabled
						? '!opacity-50 transition-none active:!scale-100'
						: ''}{doneClass ? '' + doneClass : ''}"
					onclick={() => clickFunc('done')}
				>
					{doneText}
				</button>
			{/if}
			{#each reverse ? ['1', '2', '3'] : ['7', '8', '9'] as item (item)}
				<button class={baseClassFunc(item)} onclick={() => clickFunc(item)}>{item} </button>
			{/each}
			{#if dot}
				<button class={baseClassFunc('.')} onclick={() => clickFunc('.')}>.</button>
			{/if}
			{#if done ? close : !dot && close}
				<button class={baseClassFunc('close')} onclick={() => clickFunc('close')} aria-label="close">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width={Number(height) * 2}
						height={Number(height) * 2}
						viewBox="0 0 24 24"
						class="mx-auto block fill-current"
					>
						<path
							d="M12.0001 10.0859L7.20718 5.29297L5.79297 6.70718L12.0001 12.9143L18.2072 6.70718L16.793 5.29297L12.0001 10.0859ZM18.0001 17.0001L6.00007 17.0001L6.00007 15.0001L18.0001 15.0001V17.0001Z"
						>
						</path>
					</svg>
				</button>
			{/if}
			<button class={baseClassFunc('0') + ' ' + zeroColFunc()} onclick={() => clickFunc('0')}>0</button>
			{#if !done}
				<button class={baseClassFunc('delete')} onclick={() => clickFunc('delete')} aria-label="delete">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width={Number(height) * 2}
						height={Number(height) * 2}
						viewBox="0 0 24 24"
						class="mx-auto block fill-current"
					>
						<path
							d="M6.53451 3H20.9993C21.5516 3 21.9993 3.44772 21.9993 4V20C21.9993 20.5523 21.5516 21 20.9993 21H6.53451C6.20015 21 5.88792 20.8329 5.70246 20.5547L0.369122 12.5547C0.145189 12.2188 0.145189 11.7812 0.369122 11.4453L5.70246 3.4453C5.88792 3.1671 6.20015 3 6.53451 3ZM7.06969 5L2.40302 12L7.06969 19H19.9993V5H7.06969ZM12.9993 10.5858L15.8277 7.75736L17.242 9.17157L14.4135 12L17.242 14.8284L15.8277 16.2426L12.9993 13.4142L10.1709 16.2426L8.75668 14.8284L11.5851 12L8.75668 9.17157L10.1709 7.75736L12.9993 10.5858Z"
						>
						</path>
					</svg>
				</button>
			{/if}
		</div>
	</div>
</Popup>
