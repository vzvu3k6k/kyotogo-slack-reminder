<script lang="ts">
	import { useForm, validators, required } from 'svelte-use-form';

	const times = Object.freeze(Array.from({ length: 24 }, (_, i) => `${i}:00`));

	const form = useForm({
		message: { initial: '@uji @yebis0942 明日はKyoto.goです。19:00に集まって事前確認をしましょう' },
		date: { initial: new Date().toISOString().slice(0, 10) }, // YYYY-MM-DD
		time: { initial: times[12] } // 12:00
	});

	let output = '';
	$: output = $form.valid
		? `/remind #kyoto "${$form.message.value}" at ${$form.time.value} on ${$form.date.value}`
		: '';

	let copied = false;
	async function handleClickCopy() {
		await navigator.clipboard.writeText(output);
		copied = true;
		setTimeout(() => (copied = false), 800);
	}
</script>

<svelte:head>
	<title>Kyoto.go Slack Reminder</title>
</svelte:head>

<div class="rounded-md bg-white p-4 sm:p-8">
	<h1 class="text-3xl">Kyoto.go Slack Reminder</h1>

	<form use:form class="mt-8 flex flex-col gap-4">
		<div>
			<label for="message" class="block text-sm font-medium text-gray-700">メッセージ</label>
			<div class="mt-1">
				<textarea
					id="message"
					name="message"
					rows="3"
					class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
					use:validators={[required]}
				/>
			</div>
		</div>
		<div>
			<label for="date" class="block text-sm font-medium text-gray-700">日付</label>
			<input
				type="date"
				name="date"
				id="date"
				class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				use:validators={[required]}
			/>
		</div>
		<div>
			<label for="time" class="block text-sm font-medium text-gray-700">時刻</label>
			<select
				name="time"
				id="time"
				class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				use:validators={[required]}
			>
				{#each times as time}
					<option value={time}>
						{time}
					</option>
				{/each}
			</select>
		</div>

		<div class="mt-6">
			<h2 class="block text-sm font-medium text-gray-700">コマンド</h2>
			<output class="block rounded-md bg-gray-200 mt-1 mb-2 px-4 py-5">
				{output}
			</output>
			{#if $form.valid}
				<button
					class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
					on:click|preventDefault={handleClickCopy}
				>
					{#if !copied}
						コピーする
					{:else}
						コピーしました
					{/if}</button
				>
			{:else}
				<button
					class="text-white bg-blue-400 dark:bg-blue-500 cursor-not-allowed font-medium rounded-lg text-sm px-5 py-2.5 text-center"
					disabled
				>
					コピーする</button
				>
			{/if}
		</div>
	</form>
</div>

<style lang="postcss">
	:global(.touched:invalid) {
		border-color: red;
		outline-color: red;
	}
</style>
