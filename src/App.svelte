<script>
	import { onMount } from "svelte"

	const bund = {
		en: {
			headline: 'Receipt',
			date: 'Date',
			from: 'Received From',
			desc: 'Description',
			amount: 'Amount',
			tax: 'Tax',
			total: 'Total',
			thank: 'Thank you for your business!',
			sign: "Signature",
		},
		th: {
			headline: 'ใบเสร็จรับเงิน',
			date: 'วันที่',
			from: 'รับเงินจาก',
			desc: 'รายละเอียด',
			amount: 'จำนวน',
			tax: 'ภาษี',
			total: 'รวมทั้งสิ้น',
			thank: 'ขอบคุณสำหรับการอุดหนุน!',
			sign: 'ลายเซ็นผู้รับเงิน',
		},
	}

  let pen = $state({
		lang: 'en',
    no: '20231027-001',
    date: 'October 27, 2023',
    from: 'Jane Doe',
    to: 'My company',
		toAddress: '123 Business Ave, Anytown',
    desc: 'Consultation Services',
    amount: 150.00,
    vat: 0.07,
		// theme: '',
  })

	let vat = $derived(pen.amount * (+pen.vat))
	let total = $derived(pen.amount + vat)
	let tape = $derived(bund[pen.lang] || bund.en)

	function money (value) {
		if (!isNaN(value)) {
			value = value.toLocaleString(pen.lang, { minimumFractionDigits: 2, maximumFractionDigits: 2 })
		}
		return value
	}
	function percent (value) {
		if (value % 1 !== 0) { // have decimal
			value = value.toFixed(2)
		}
		return value + '%'
	}

	function finish () {
    const pass = new URLSearchParams()
		for (const key in pen) {
			let value = pen[key]
			if (value) {
				pass.append(key, value)
			}
		}
    navigator.clipboard.writeText("https://codepen.io/zummon/full/ogvoyzN?" + pass.toString())
  }
	onMount(() => {
		const pass = new URLSearchParams(window.location.search)
		for (const key in pen) {
			let value = pass.get(key)
			if (value) {
				if (typeof pen[key] == 'number') {
					value = +value
				}
				pen[key] = value
			}
		}
	})
</script>

<div class="flex justify-center gap-4 mt-5 print:hidden">
	<div class="">
		<button class="bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1" title="Saved link will be copied to clipboard" onclick={() => {
			finish()
		}}>
			<!-- https://flowbite.com/icons/ floppy-disk -->
			<svg class="w-[32px] h-[32px]" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
				<path fill-rule="evenodd" d="M5 3a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V7.414A2 2 0 0 0 20.414 6L18 3.586A2 2 0 0 0 16.586 3H5Zm10 11a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM8 7V5h8v2a1 1 0 0 1-1 1H9a1 1 0 0 1-1-1Z" clip-rule="evenodd"/>
			</svg>		
		</button>
	</div>
	<div class="">
		<button class="bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1" title="Print" onclick={() => {
			print()
		}}>
			<!-- https://flowbite.com/icons/ printer -->
			<svg class="w-[32px] h-[32px]" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
				<path fill-rule="evenodd" d="M8 3a2 2 0 0 0-2 2v3h12V5a2 2 0 0 0-2-2H8Zm-3 7a2 2 0 0 0-2 2v5a2 2 0 0 0 2 2h1v-4a1 1 0 0 1 1-1h10a1 1 0 0 1 1 1v4h1a2 2 0 0 0 2-2v-5a2 2 0 0 0-2-2H5Zm4 11a1 1 0 0 1-1-1v-4h8v4a1 1 0 0 1-1 1H9Z" clip-rule="evenodd"/>
			</svg>					
		</button>
	</div>
	<!-- <div class="">
		<input class="" type="color" bind:value={pen.theme}>
	</div> -->
	<div class="">
		<select class="text-teal-500 rounded-lg shadow-md appearance-none uppercase py-1 px-2 cursor-pointer" bind:value={pen.lang}>
			{#each Object.keys(bund) as value}
				<option>{value}</option>
			{/each}
		</select>
	</div>
</div>

<div class="mt-5 print:hidden text-center">
	Select text you want to edit then type directly
</div>

<div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md shadow-blue-200 mt-3 border-t-8 border-blue-500" style='font-family: "Cabin", serif;'>
  <div class="flex justify-between items-center mb-4">
    <div>
      <h2 class="text-xl font-bold text-black">{tape.headline}</h2>
      <p class="text-blue-500">#<span contenteditable bind:textContent={pen.no}></span></p>
    </div>
    <div class="">
      <h3 class="border-b-2 border-blue-500 font-semibold text-xl w-fit ml-auto" contenteditable bind:textContent={pen.to}></h3>
			<p class="text-sm" contenteditable bind:textContent={pen.toAddress}></p>
    </div>
  </div>

  <div class="mb-4">
    <div class="flex justify-between">
      <span class="">{tape.date}:</span>
      <span class="" contenteditable bind:textContent={pen.date}></span>
    </div>
    <div class="flex justify-between">
      <span class="">{tape.from}:</span>
      <span class="" contenteditable bind:textContent={pen.from}></span>
    </div>
  </div>

  <div class="border-t border-b border-blue-400 py-3 my-3">
    <div class="flex justify-between mb-1">
      <span class="">{tape.desc}:</span>
      <span class="" contenteditable bind:textContent={pen.desc}></span>
    </div>
    <div class="flex justify-between">
      <span class="">{tape.amount}:</span>
      <span class="" contenteditable onfocus={(e) => {
				e.target.textContent = pen.amount
			}} oninput={(e) => {
				pen.amount = +e.target.textContent
			}} onblur={(e) => {
				e.target.textContent = money(pen.amount)
			}}>{money(pen.amount)}</span>
    </div>
  </div>

  <div class="font-medium">
    <div class="flex justify-between">
      <span class="">{tape.tax} (<span contenteditable onfocus={(e) => {
				e.target.textContent = pen.vat
			}} oninput={(e) => {
				pen.vat = +e.target.textContent
			}} onblur={(e) => {
				e.target.textContent = percent(pen.vat * 100)
			}}>{percent(pen.vat * 100)}</span>):</span>
      <span class="">{money(vat)}</span>
    </div>
    <div class="flex justify-between font-bold text-lg mt-2">
      <span class="text-black">{tape.total}:</span>
      <span class="text-black">{money(total)}</span>
    </div>
  </div>

  <div class="mt-6 text-center">
		<p class="">{tape.sign}</p>
		<br>
		<br>
    <p class="text-blue-500 border-t border-blue-500 w-fit mx-auto pt-1">{tape.thank}</p>
  </div>
</div>

<!-- <pre class="w-fit mx-auto print:hidden">{JSON.stringify(pen, null, 2)}</pre> -->