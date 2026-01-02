<script>
	import { onMount } from "svelte";
	const bund = {
		en: {
			family: '"Cabin", serif',
			headline: "Receipt",
			date: "Date",
			from: "Received From",
			desc: "Description",
			amount: "Amount",
			tax: "Tax",
			total: "Total",
			thank: "Thank you for your business!",
			sign: "Signature",
		},
		th: {
			family: '"Sarabun", sans-serif',
			headline: "ใบเสร็จรับเงิน",
			date: "วันที่",
			from: "รับเงินจาก",
			desc: "รายละเอียด",
			amount: "จำนวน",
			tax: "ภาษี",
			total: "รวมทั้งสิ้น",
			thank: "ขอบคุณสำหรับการอุดหนุน!",
			sign: "ลายเซ็นผู้รับเงิน",
		},
		es: {
			// Spanish
			headline: "Recibo",
			date: "Fecha",
			from: "Recibido De",
			desc: "Descripción",
			amount: "Cantidad",
			tax: "Impuesto",
			total: "Total",
			thank: "¡Gracias por su compra!", // Or "¡Gracias por su negocio!"
			sign: "Firma",
		},
		fr: {
			// French
			headline: "Reçu",
			date: "Date",
			from: "Reçu De",
			desc: "Description",
			amount: "Montant",
			tax: "Taxe", // Or "Impôt" (more formal)
			total: "Total",
			thank: "Merci pour votre achat !", // Or "Merci de votre confiance !"
			sign: "Signature",
		},
		de: {
			// German
			headline: "Quittung",
			date: "Datum",
			from: "Erhalten Von",
			desc: "Beschreibung",
			amount: "Betrag",
			tax: "Steuer",
			total: "Gesamt",
			thank: "Vielen Dank für Ihren Einkauf!",
			sign: "Unterschrift",
		},
		ja: {
			// Japanese
			headline: "領収書 (Ryōshūsho)",
			date: "日付 (Hizuke)",
			from: "受取人 (Uketorinin)", // Or "受領者 (Juryōsha)"
			desc: "説明 (Setsumei)",
			amount: "金額 (Kingaku)",
			tax: "税 (Zei)",
			total: "合計 (Gōkei)",
			thank: "ご利用ありがとうございます (Go riyō arigatō gozaimasu)", // More polite: 誠にありがとうございます (Makoto ni arigatō gozaimasu)
			sign: "署名 (Shomei)",
		},
		"zh-CN": {
			// Chinese (Simplified)
			headline: "收据 (Shōujù)",
			date: "日期 (Rìqī)",
			from: "收款人 (Shōukuǎn rén)", // Or "收到方 (Shōudào fāng)"
			desc: "描述 (Miáoshù)",
			amount: "金额 (Jīn'é)",
			tax: "税 (Shuì)",
			total: "总计 (Zǒngjì)",
			thank: "感谢您的惠顾！(Gǎnxiè nín de huìgù!)", // Or "谢谢您的光临！(Xièxie nín de guānglín!)"
			sign: "签名 (Qiānmíng)",
		},
	};
	const penStruct = {
		no: "20231027-002",
		date: "October 27, 2023",
		from: "Mac Donel",
		desc: "Consultation Services",
		amount: 80.0,
	};

	let store = $state({
		logo: "",
	});
	let tale = $state({
		theme: "#3b82f6",
		lang: "en",
		vat: 0.07,
		to: "My company",
		toAddress: "444 Globe, Anytown",
	});
	let pens = $state([{ ...penStruct }]);
	let savedUrl = $state("");

	let tape = $derived({ ...bund.en, ...bund[tale.lang] });

	function money(value) {
		if (!isNaN(value)) {
			value = value.toLocaleString(tale.lang, {
				minimumFractionDigits: 2,
				maximumFractionDigits: 2,
			});
		}
		return value;
	}
	function percent(value) {
		if (value % 1 !== 0) {
			// have decimal
			value = value.toFixed(2);
		}
		return value + "%";
	}
	function strToArr(str) {
		let aoa = [];
		let result = [];
		if (str) {
			str.split("\n").forEach((row, rowindex) => {
				aoa[rowindex] = [];
				row.split("\t").forEach((col, colindex) => {
					aoa[rowindex][colindex] = col;
				});
			});
			aoa.slice(1).forEach((row, rowindex) => {
				result[rowindex] = {};
				aoa[0].forEach((key, colindex) => {
					if (penStruct[key]) {
						if (typeof penStruct[key] == "number") {
							result[rowindex][key] = Number(row[colindex]);
						} else {
							result[rowindex][key] = row[colindex];
						}
					}
				});
			});
			pens = result;
		} else {
			pens[0] = { ...penStruct };
		}
	}

	function upload(e) {
		const file = e.target.files[0];
		if (file) {
			const reader = new FileReader();
			reader.addEventListener("load", () => {
				store.logo = reader.result;
				localStorage.setItem("logo", store.logo);
			});
			reader.readAsDataURL(file);
		} else {
			store.logo = "";
			localStorage.setItem("logo", "");
		}
	}
	function finish() {
		if (savedUrl) {
			savedUrl = "";
		} else {
			const pass = new URLSearchParams();
			for (const key in tale) {
				let value = tale[key];
				if (value) {
					pass.append(key, value);
				}
			}
			let index = 0;
			for (const pen of pens) {
				for (let key in pen) {
					let value = pen[key];
					if (value) {
						if (typeof pens[0][key] == "number") {
							value = Number(value);
						}
						pass.append(key + index, value);
					}
				}
				index += 1;
			}
			savedUrl = "https://codepen.io/zummon/full/ogvoyzN?" + pass.toString();
			navigator.clipboard.writeText(savedUrl);
		}
	}
	function start() {
		const params = new URLSearchParams(location.search);
		for (let [key, value] of params.entries()) {
			if (tale[key]) {
				if (typeof tale[key] == "number") {
					value = Number(value);
				}
				tale[key] = value;
			}
			let slug = key.replace(/\d+$/, "");
			let index = key.match(/\d+$/)?.[0];
			if (pens[0][slug]) {
				if (typeof pens[0][slug] == "number") {
					value = Number(value);
				}
				if (!pens[index]) {
					pens[index] = {};
				}
				pens[index][slug] = value;
			}
		}
		const logo = localStorage.getItem("logo");
		if (logo) {
			store.logo = logo;
		}
	}
	onMount(() => {
		start();
	});
</script>

<div class="flex flex-wrap justify-center gap-4 mt-5 print:hidden">
	<div class="">
		<button
			class="cursor-pointer bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1"
			title="input data"
			onclick={() => {}}
		>
			<!-- https://flowbite.com/icons/ database -->
			<svg
				class="size-8"
				aria-hidden="true"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				fill="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					d="M12 7.205c4.418 0 8-1.165 8-2.602C20 3.165 16.418 2 12 2S4 3.165 4 4.603c0 1.437 3.582 2.602 8 2.602ZM12 22c4.963 0 8-1.686 8-2.603v-4.404c-.052.032-.112.06-.165.09a7.75 7.75 0 0 1-.745.387c-.193.088-.394.173-.6.253-.063.024-.124.05-.189.073a18.934 18.934 0 0 1-6.3.998c-2.135.027-4.26-.31-6.3-.998-.065-.024-.126-.05-.189-.073a10.143 10.143 0 0 1-.852-.373 7.75 7.75 0 0 1-.493-.267c-.053-.03-.113-.058-.165-.09v4.404C4 20.315 7.037 22 12 22Zm7.09-13.928a9.91 9.91 0 0 1-.6.253c-.063.025-.124.05-.189.074a18.935 18.935 0 0 1-6.3.998c-2.135.027-4.26-.31-6.3-.998-.065-.024-.126-.05-.189-.074a10.163 10.163 0 0 1-.852-.372 7.816 7.816 0 0 1-.493-.268c-.055-.03-.115-.058-.167-.09V12c0 .917 3.037 2.603 8 2.603s8-1.686 8-2.603V7.596c-.052.031-.112.059-.165.09a7.816 7.816 0 0 1-.745.386Z"
				/>
			</svg>
		</button>
	</div>
	<div class="">
		<button
			class="cursor-pointer bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1"
			title="Saved link will be copied to clipboard"
			onclick={() => {
				finish();
			}}
		>
			<!-- https://flowbite.com/icons/ floppy-disk -->
			<svg
				class="size-8"
				aria-hidden="true"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				fill="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					fill-rule="evenodd"
					d="M5 3a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V7.414A2 2 0 0 0 20.414 6L18 3.586A2 2 0 0 0 16.586 3H5Zm10 11a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM8 7V5h8v2a1 1 0 0 1-1 1H9a1 1 0 0 1-1-1Z"
					clip-rule="evenodd"
				/>
			</svg>
		</button>
	</div>
	<div class="">
		<button
			class="cursor-pointer bg-teal-500 text-white rounded-lg shadow-md shadow-teal-200 p-1"
			title="Print"
			onclick={() => {
				print();
			}}
		>
			<!-- https://flowbite.com/icons/ printer -->
			<svg
				class="size-8"
				aria-hidden="true"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				fill="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					fill-rule="evenodd"
					d="M8 3a2 2 0 0 0-2 2v3h12V5a2 2 0 0 0-2-2H8Zm-3 7a2 2 0 0 0-2 2v5a2 2 0 0 0 2 2h1v-4a1 1 0 0 1 1-1h10a1 1 0 0 1 1 1v4h1a2 2 0 0 0 2-2v-5a2 2 0 0 0-2-2H5Zm4 11a1 1 0 0 1-1-1v-4h8v4a1 1 0 0 1-1 1H9Z"
					clip-rule="evenodd"
				/>
			</svg>
		</button>
	</div>
	<div class="">
		<select
			class="text-center text-teal-500 rounded-lg shadow-md uppercase py-1 px-2 cursor-pointer font-bold bg-white appearance-none field-sizing-content"
			title="Change language"
			bind:value={tale.lang}
		>
			{#each Object.keys(bund) as value}
				<option class="text-black">{value}</option>
			{/each}
		</select>
	</div>
	<div class="">
		<input class="" type="color" bind:value={tale.theme} />
	</div>
</div>

<div class="mt-5 mb-3 print:hidden text-center px-4">
	{#if true}
		<div class="">
			<textarea
				class="border w-full"
				placeholder="no	date	from	desc	amount
20231027-002	October 27, 2023	Mac Donel	Consultation Services	80.0"
				onchange={(e) => {
					strToArr(e.currentTarget.value);
				}}
			></textarea>
		</div>
	{/if}
	{#if savedUrl}
		<div class="truncate">
			<a class="text-teal-500" target="_top" href={savedUrl}>{savedUrl}</a>
		</div>
	{/if}
	Select text you want to edit then type directly.<br />
	Click the logo to upload yours
</div>

<div
	class="flex flex-wrap justify-center items-center gap-6"
	style:font-family={tape.family}
>
	{#each pens as pen, index (index)}
		{@const vat = pen.amount * Number(tale.vat)}
		{@const total = pen.amount + vat}
		<div class="relative">
			<button
				class="print:hidden cursor-pointer absolute text-pink-500 right-0 bg-white rounded-lg shadow-md"
				onclick={() => {
					pens.splice(index, 1);
				}}
			>
				<!-- https://flowbite.com/icons/ close -->
				<svg
					class="size-8"
					aria-hidden="true"
					xmlns="http://www.w3.org/2000/svg"
					width="24"
					height="24"
					fill="none"
					viewBox="0 0 24 24"
				>
					<path
						stroke="currentColor"
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M6 18 17.94 6M18 18 6.06 6"
					/>
				</svg>
			</button>
			<div
				class="w-md p-6 bg-white rounded-lg shadow-md border-t-8 break-inside-avoid"
				style:border-color={tale.theme}
			>
				<div class="mx-auto w-fit">
					<label class="cursor-pointer" title="Upload your logo">
						<input
							class="hidden"
							type="file"
							onchange={(e) => {
								upload(e);
							}}
						/>
						<img
							class="max-h-20 max-w-full {store.logo ? '' : 'print:hidden'}"
							src={store.logo}
							alt="Logo"
						/>
					</label>
				</div>

				<div class="flex justify-between items-center mb-4">
					<div>
						<h2 class="text-xl font-bold text-black">{tape.headline}</h2>
						<p class="" style:color={tale.theme}>
							#<span
								class="bg-yellow-200 print:bg-transparent p-1 print:p-0"
								contenteditable
								bind:textContent={pen.no}
							></span>
						</p>
					</div>
					<div class="">
						<h3
							class="border-b-2 font-semibold text-xl w-fit ml-auto bg-yellow-200 print:bg-transparent"
							style:border-color={tale.theme}
							contenteditable
							bind:textContent={tale.to}
						></h3>
						<p
							class="text-sm bg-yellow-200 print:bg-transparent p-1 print:p-0"
							contenteditable
							bind:textContent={tale.toAddress}
						></p>
					</div>
				</div>

				<div class="mb-4">
					<div class="flex justify-between">
						<span class="">{tape.date}:</span>
						<span
							class="bg-yellow-200 print:bg-transparent p-1 print:p-0"
							contenteditable
							bind:textContent={pen.date}
						></span>
					</div>
					<div class="flex justify-between">
						<span class="">{tape.from}:</span>
						<span
							class="bg-yellow-200 print:bg-transparent p-1 print:p-0"
							contenteditable
							bind:textContent={pen.from}
						></span>
					</div>
				</div>

				<div
					class="border-t border-b py-3 my-3"
					style:border-color={tale.theme}
				>
					<div class="flex justify-between mb-1">
						<span class="">{tape.desc}:</span>
						<span
							class="bg-yellow-200 print:bg-transparent p-1 print:p-0"
							contenteditable
							bind:textContent={pen.desc}
						></span>
					</div>
					<div class="flex justify-between">
						<span class="">{tape.amount}:</span>
						<span
							class="bg-yellow-200 print:bg-transparent p-1 print:p-0"
							contenteditable
							onfocus={(e) => {
								e.currentTarget.textContent = pen.amount;
							}}
							oninput={(e) => {
								pen.amount = Number(e.currentTarget.textContent);
							}}
							onblur={(e) => {
								e.currentTarget.textContent = money(pen.amount);
							}}>{money(pen.amount)}</span
						>
					</div>
				</div>

				<div class="font-medium">
					<div class="flex justify-between">
						<span class=""
							>{tape.tax} (<span
								class="bg-yellow-200 print:bg-transparent"
								contenteditable
								onfocus={(e) => {
									e.currentTarget.textContent = tale.vat;
								}}
								oninput={(e) => {
									tale.vat = Number(e.currentTarget.textContent);
								}}
								onblur={(e) => {
									e.currentTarget.textContent = percent(tale.vat * 100);
								}}>{percent(tale.vat * 100)}</span
							>):</span
						>
						<span class="">{money(vat)}</span>
					</div>
					<div class="flex justify-between font-bold text-lg mt-2">
						<span class="text-black">{tape.total}:</span>
						<span class="text-black">{money(total)}</span>
					</div>
				</div>

				<div class="mt-6 text-center">
					<p class="">{tape.sign}</p>
					<br />
					<br />
					<p
						class="border-t w-fit mx-auto pt-1"
						style:color={tale.theme}
						style:border-color={tale.theme}
					>
						{tape.thank}
					</p>
				</div>
			</div>
		</div>
	{/each}
	<div class="print:hidden">
		<button
			class="cursor-pointer text-teal-500"
			onclick={() => {
				pens.push({});
			}}
		>
			<!-- https://flowbite.com/icons/ plus -->
			<svg
				class="size-12"
				aria-hidden="true"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				fill="none"
				viewBox="0 0 24 24"
			>
				<path
					stroke="currentColor"
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M5 12h14m-7 7V5"
				/>
			</svg>
		</button>
	</div>
</div>
