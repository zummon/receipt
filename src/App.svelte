<script>
	import { onMount } from "svelte"

  let pen = $state({
    no: '20231027-001',
    date: 'October 27, 2023',
    from: 'Jane Doe',
    to: 'My company',
    desc: 'Consultation Services',
    amount: 150.00,
    vat: 0.07,
		// theme: '',
  })

	let vat = $derived(pen.amount * (+pen.vat))
	let total = $derived(pen.amount + vat)

	function money (value, option) {
		if (!isNaN(value)) {
			value = value.toLocaleString(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2, ...option })
		}
		return value
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
		const pass = new URLSearchParams(window.location.search);
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
</div>

<div class="mt-5 print:hidden text-center">
	Select text you want to edit then type directly
</div>

<div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md shadow-blue-200 mt-3 border-t-8 border-blue-500">
  <div class="flex justify-between items-center mb-4">
    <div>
      <h2 class="text-xl font-bold text-black">Cash Receipt</h2>
      <p class="text-sm text-blue-500">#<span contenteditable bind:textContent={pen.no}></span></p>
    </div>
    <div>
      <div class="border-b-2 border-blue-500 font-semibold text-xl" contenteditable bind:textContent={pen.to}>
      </div>
    </div>
  </div>

  <div class="mb-4 text-sm">
    <div class="flex justify-between">
      <span class="">Date:</span>
      <span class="" contenteditable bind:textContent={pen.date}></span>
    </div>
    <div class="flex justify-between">
      <span class="">Received From:</span>
      <span class="" contenteditable bind:textContent={pen.from}></span>
    </div>
  </div>

  <div class="border-t border-b border-blue-400 py-3 my-3 text-sm">
    <div class="flex justify-between mb-1">
      <span class="">Description:</span>
      <span class="" contenteditable bind:textContent={pen.desc}></span>
    </div>
    <div class="flex justify-between">
      <span class="">Amount:</span>
      <span class="" contenteditable onfocus={(e) => {
				e.target.textContent = pen.amount
			}} oninput={(e) => {
				pen.amount = +e.target.textContent
			}} onblur={(e) => {
				e.target.textContent = money(pen.amount)
			}}>{money(pen.amount)}</span>
    </div>
  </div>

  <div class="text-sm font-medium">
    <div class="flex justify-between">
      <span class="">Tax (<span contenteditable onfocus={(e) => {
				e.target.textContent = pen.vat
			}} oninput={(e) => {
				pen.vat = +e.target.textContent
			}} onblur={(e) => {
				e.target.textContent = money(pen.vat * 100) + '%'
			}}>{money(pen.vat * 100)}%</span>):</span>
      <span class="">{money(vat)}</span>
    </div>
    <div class="flex justify-between font-bold text-lg mt-2">
      <span class="text-black">Total:</span>
      <span class="text-black">{money(total)}</span>
    </div>
  </div>

  <div class="mt-6 text-center text-xs text-blue-500">
    <p>Thank you for your business!</p>
    <p>123 Business Ave, Anytown</p>
  </div>
</div>

<!-- <pre class="w-fit mx-auto print:hidden">{JSON.stringify(pen, null, 2)}</pre> -->