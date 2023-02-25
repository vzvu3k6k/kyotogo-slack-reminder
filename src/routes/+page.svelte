<script lang="ts">
	const times = Array.from({ length: 24 }, (_, i) => `${i}:00`);

	let form = {
		message: '@uji @yebis0942 明日はKyoto.goです。19:00に集まって事前確認をしましょう',
		date: '',
		time: times[12], // 12:00
	}

	let validationErrors: string[] = [];

	let output = '';
	$: output = form.date ? `/remind #kyoto "${form.message}" at ${form.time} on ${form.date}` : '日付を入力してください';

	let copied = false;
	async function handleClickCopy() {
		await navigator.clipboard.writeText(output);
		copied = true;
		setTimeout(() => (copied = false), 800);
	}
</script>

<div class="rounded-md bg-white px-4 py-5 sm:p-6">
	<h1 class="text-3xl">Kyoto.go Slack Reminder</h1>

	<form class="mt-8 flex flex-col gap-4">
		<div>
			<label for="message" class="block text-sm font-medium text-gray-700">メッセージ</label>
			<div class="mt-1">
				<textarea
					id="message"
					name="message"
					rows="3"
					class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
					bind:value={form.message}
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
				bind:value={form.date}
			/>
		</div>
		<div>
			<label for="time" class="block text-sm font-medium text-gray-700">時刻</label>
			<select
				name="time"
				id="time"
				class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				bind:value={form.time}
			>
				{#each times as time}
					<option value={time}>
						{time}
					</option>
				{/each}
			</select>
		</div>

		<div>
			<h2 class="block text-sm font-medium text-gray-700">コマンド</h2>
			<output class="block rounded-md bg-gray-200 mt-1 px-4 py-5">
				{output}
			</output>
			<button
				class="mt-2 inline-flex justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
				on:click|preventDefault={handleClickCopy}
			>
				{#if !copied}
					コピーする
				{:else}
					コピーしました
				{/if}</button
			>
		</div>
	</form>
</div>

<style lang="postcss">
	:global(html) {
		background-color: theme(colors.gray.100);
	}
</style>
