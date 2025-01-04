<script>
	import { onMount } from "svelte"

  let pen = $state({
    no: '20231027-001',
    date: 'October 27, 2023',
    from: 'Jane Doe',
    to: 'AC',
    desc: 'Consultation Services',
    amount: 150.00,
    vat: 0.07,
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
    const searchParams = new URLSearchParams()
		for (const key in pen) {
			let value = pen[key]
			searchParams.append(key, value)
		}
    navigator.clipboard.writeText(
      "https://codepen.io/zummon/full/ogvoyzN?" + searchParams.toString()
    )
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
	<button class="bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1" onclick={() => {
		finish()
	}}>
		<!-- https://fonts.google.com/icons?selected=Material+Symbols+Rounded:save:FILL@1;wght@400;GRAD@0;opsz@40&icon.query=save&icon.style=Rounded&icon.size=32&icon.color=currentColor -->
		<svg xmlns="http://www.w3.org/2000/svg" height="40px" viewBox="0 -960 960 960" width="40px" fill="currentColor"><path d="M186.67-120q-27 0-46.84-19.83Q120-159.67 120-186.67v-586.66q0-27 19.83-46.84Q159.67-840 186.67-840h467.66q13.63 0 25.98 5.33 12.36 5.34 21.36 14.34l118.66 118.66q9 9 14.34 21.36 5.33 12.35 5.33 25.98v467.66q0 27-19.83 46.84Q800.33-120 773.33-120H186.67Zm293.17-123.33q45.49 0 77.49-31.85 32-31.84 32-77.33 0-45.49-31.84-77.49-31.84-32-77.33-32-45.49 0-77.49 31.84-32 31.85-32 77.34t31.84 77.49q31.84 32 77.33 32ZM268.67-576h292q14.16 0 23.75-9.58 9.58-9.59 9.58-23.75v-82q0-14.17-9.58-23.75-9.59-9.59-23.75-9.59h-292q-14.17 0-23.75 9.59-9.59 9.58-9.59 23.75v82q0 14.16 9.59 23.75 9.58 9.58 23.75 9.58Z"/></svg>
	</button>
	<button class="bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1" onclick={() => {
		print()
	}}>
		<!-- https://fonts.google.com/icons?selected=Material+Symbols+Rounded:print:FILL@1;wght@400;GRAD@0;opsz@40&icon.query=print&icon.style=Rounded&icon.size=32&icon.color=currentColor -->
		<svg xmlns="http://www.w3.org/2000/svg" height="40px" viewBox="0 -960 960 960" width="40px" fill="currentColor"><path d="M308-120q-27.5 0-47.08-19.58-19.59-19.59-19.59-47.09v-104h-94.66q-27.5 0-47.09-19.58Q80-329.83 80-357.33v-177.34q0-47.03 32-78.85 32-31.81 78.67-31.81h578.66q47.04 0 78.85 31.81Q880-581.7 880-534.67v177.34q0 27.5-19.58 47.08-19.59 19.58-47.09 19.58h-94.66v104q0 27.5-19.59 47.09Q679.5-120 652-120H308Zm410.67-558.67H241.33v-94.66q0-27.5 19.59-47.09Q280.5-840 308-840h344q27.5 0 47.08 19.58 19.59 19.59 19.59 47.09v94.66Zm14 196.67q13.66 0 23.5-9.83 9.83-9.84 9.83-23.5 0-13.67-9.83-23.5-9.84-9.84-23.5-9.84-13.67 0-23.5 9.84-9.84 9.83-9.84 23.5 0 13.66 9.84 23.5Q719-482 732.67-482ZM308-186.67h344V-368H308v181.33Z"/></svg>
	</button>
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
			}} onblur={(e) => {
				pen.amount = +e.target.textContent
				e.target.textContent = money(pen.amount)
			}}>{money(pen.amount)}</span>
    </div>
  </div>

  <div class="text-sm font-medium">
    <div class="flex justify-between">
      <span class="">Tax (<span contenteditable onfocus={(e) => {
				e.target.textContent = pen.vat
			}} onblur={(e) => {
				pen.vat = +e.target.textContent
				e.target.textContent = money(pen.vat * 100)
			}}>{money(pen.vat * 100)}</span>%):</span>
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