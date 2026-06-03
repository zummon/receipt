<script>
	import { onMount } from "svelte";

	const bund = {
		en: {
			family: "'Courier Prime', monospace",
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
			family: "'Sarabun', sans-serif",
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
			family: "'Courier Prime', monospace",
			headline: "Recibo",
			date: "Fecha",
			from: "Recibido De",
			desc: "Descripción",
			amount: "Cantidad",
			tax: "Impuesto",
			total: "Total",
			thank: "¡Gracias por su compra!",
			sign: "Firma",
		},
		fr: {
			family: "'Courier Prime', monospace",
			headline: "Reçu",
			date: "Date",
			from: "Reçu De",
			desc: "Description",
			amount: "Montant",
			tax: "Taxe",
			total: "Total",
			thank: "Merci pour votre achat !",
			sign: "Signature",
		},
		de: {
			family: "'Courier Prime', monospace",
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
			family: "'Noto Sans JP', sans-serif",
			headline: "領収書",
			date: "日付",
			from: "受取人",
			desc: "説明",
			amount: "金額",
			tax: "税",
			total: "合計",
			thank: "ご利用ありがとうございます",
			sign: "署名",
		},
		"zh-CN": {
			family: "'Noto Sans SC', sans-serif",
			headline: "收据",
			date: "日期",
			from: "收款人",
			desc: "描述",
			amount: "金额",
			tax: "税",
			total: "总计",
			thank: "感谢您的惠顾！",
			sign: "签名",
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
		theme: "#2d6a4f", // deep forest green default
		lang: "en",
		vat: 0.07,
		to: "My company",
		toAddress: "444 Globe, Anytown",
		currency: "$",
	});

	let pens = $state([{ ...penStruct }]);
	let savedUrl = $state("");
	let showDataInput = $state(false);

	let tape = $derived({ ...bund.en, ...bund[tale.lang] });

	let overallTotal = $derived(
		pens.reduce((sum, pen) => {
			const amt = Number(pen.amount) || 0;
			const vt = amt * Number(tale.vat);
			return sum + amt + vt;
		}, 0)
	);

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
					let cleanKey = key.trim();
					if (penStruct[cleanKey] !== undefined) {
						if (typeof penStruct[cleanKey] == "number") {
							result[rowindex][cleanKey] = Number(row[colindex]);
						} else {
							result[rowindex][cleanKey] = row[colindex];
						}
					}
				});
			});
			pens = result;
		} else {
			pens = [{ ...penStruct }];
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
				if (value !== undefined && value !== null && value !== "") {
					pass.append(key, value);
				}
			}
			let index = 0;
			for (const pen of pens) {
				for (let key in pen) {
					let value = pen[key];
					if (value !== undefined && value !== null) {
						if (typeof penStruct[key] == "number") {
							value = Number(value);
						}
						pass.append(key + index, value);
					}
				}
				index += 1;
			}
			const baseUrl = window.location.origin + window.location.pathname;
			savedUrl = baseUrl + "?" + pass.toString();
			navigator.clipboard.writeText(savedUrl);
		}
	}

	function start() {
		const params = new URLSearchParams(location.search);
		let hasParams = false;
		for (const key of params.keys()) {
			let slug = key.replace(/\d+$/, "");
			if (penStruct[slug] !== undefined) {
				hasParams = true;
				break;
			}
		}

		if (hasParams) {
			pens = [];
		}

		for (let [key, value] of params.entries()) {
			if (tale[key] !== undefined) {
				if (typeof tale[key] == "number") {
					tale[key] = Number(value);
				} else {
					tale[key] = value;
				}
			}
			let slug = key.replace(/\d+$/, "");
			let index = key.match(/\d+$/)?.[0];
			if (index !== undefined) {
				index = Number(index);
				if (penStruct[slug] !== undefined) {
					if (!pens[index]) {
						pens[index] = { ...penStruct };
					}
					if (typeof penStruct[slug] == "number") {
						pens[index][slug] = Number(value);
					} else {
						pens[index][slug] = value;
					}
				}
			}
		}

		if (pens.length === 0) {
			pens = [{ ...penStruct }];
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

<svelte:head>
	{#if tale.lang === 'th'}
		<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
	{:else if tale.lang === 'ja'}
		<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&display=swap" rel="stylesheet">
	{:else if tale.lang === 'zh-CN'}
		<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
	{:else}
		<link href="https://fonts.googleapis.com/css2?family=Courier+Prime:wght@400;700&family=Cabin:wght@400;500;600;700&display=swap" rel="stylesheet">
	{/if}
</svelte:head>

<div class="app-container">
	<!-- Sidebar (Dashboard) -->
	<aside class="sidebar print-hidden">
		<h2 class="sidebar-title">Dashboard</h2>
		
		<div class="sidebar-metric">
			<span class="sidebar-metric-label">Total Receipts</span>
			<span class="sidebar-metric-value">{pens.length}</span>
		</div>
		
		<div class="sidebar-metric">
			<span class="sidebar-metric-label">Overall Total</span>
			<span class="sidebar-metric-value" style:color={tale.theme}>{tale.currency || ''} {money(overallTotal)}</span>
		</div>
	</aside>

	<!-- Main Content Area -->
	<main class="main-content">
		<!-- Control Bar -->
		<div class="control-bar print-hidden">
			<div class="control-item">
				<button
					class="btn btn-teal"
					title="Input data (TSV format)"
					onclick={() => { showDataInput = !showDataInput; }}
				>
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
			
			<div class="control-item">
				<button
					class="btn btn-teal"
					title="Copy shareable link to clipboard"
					onclick={finish}
				>
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
			
			<div class="control-item">
				<button
					class="btn btn-teal"
					title="Print receipts"
					onclick={() => print()}
				>
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

			<div class="control-item">
				<label class="btn btn-teal" title="Upload Logo" style="cursor: pointer;">
					<input
						style="display: none;"
						type="file"
						onchange={upload}
					/>
					<svg
						class="size-8"
						aria-hidden="true"
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						fill="currentColor"
						viewBox="0 0 24 24"
					>
						<path fill-rule="evenodd" d="M13 10a1 1 0 0 1-1-1V4H8v6a1 1 0 0 1-1 1H3v10h18V10h-8Zm1 0h5.586L14 4.414V10ZM3 8h4V4H3v4Zm0 14h18a2 2 0 0 0 2-2V8a2 2 0 0 0-2-2h-5V3a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1v3H3a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2Z" clip-rule="evenodd"/>
					</svg>
				</label>
			</div>

			<div class="control-item">
				<select
					class="select-input"
					title="Change language"
					bind:value={tale.lang}
				>
					{#each Object.keys(bund) as value}
						<option value={value}>{value.toUpperCase()}</option>
					{/each}
				</select>
			</div>

			<div class="control-item">
				<div class="color-picker-wrapper" style:background-color={tale.theme} title="Theme Color">
					<input class="color-input" type="color" bind:value={tale.theme} />
				</div>
			</div>
		</div>

		<!-- Data Input Area -->
		{#if showDataInput}
			<div class="data-input-container print-hidden">
				<textarea
					class="data-textarea"
					placeholder="Paste tab-separated data here (columns: no, date, from, desc, amount)&#13;Example:&#13;no	date	from	desc	amount&#13;20231027-002	October 27, 2023	Mac Donel	Consultation Services	80.0"
					onchange={(e) => strToArr(e.currentTarget.value)}
				></textarea>
			</div>
		{/if}

		<!-- Shareable URL feedback -->
		{#if savedUrl}
			<div class="alert-link print-hidden">
				<p>Link copied to clipboard! Share or bookmark this URL:</p>
				<a target="_top" href={savedUrl}>{savedUrl}</a>
			</div>
		{/if}

		<!-- Receipts Display Container -->
		<div
			class="receipts-container"
			style:font-family={tape.family}
			class:lang-th={tale.lang === 'th'}
			class:lang-ja={tale.lang === 'ja'}
			class:lang-zh={tale.lang === 'zh-CN'}
		>
			{#each pens as pen, index (index)}
				{@const vat = pen.amount * Number(tale.vat)}
				{@const total = pen.amount + vat}
				<div class="receipt-card page-break" style:--theme-color={tale.theme}>
					<!-- Delete button -->
					<button
						class="btn-delete print-hidden"
						title="Delete receipt"
						onclick={() => pens.splice(index, 1)}
					>
						<svg
							class="size-5"
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

					<!-- Background SVG Organic Watermark Shapes (Adjustable color) -->
					<svg class="bg-shape shape-fern" viewBox="0 0 100 100" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round">
						<path d="M50 90 Q45 60 50 10 M50 40 Q70 30 85 35 M50 50 Q30 45 15 50 M50 60 Q75 55 85 62 M50 70 Q25 65 15 72" />
					</svg>
					<svg class="bg-shape shape-leaf" viewBox="0 0 100 100" fill="currentColor">
						<path d="M50 90 C50 90 85 60 85 35 C85 15 65 10 50 35 C35 10 15 15 15 35 C15 60 50 90 50 90 Z" />
					</svg>

					<!-- Logo Display -->
					{#if store.logo}
						<div class="logo-container">
							<img
								class="logo-img"
								src={store.logo}
								alt="Logo"
							/>
						</div>
					{/if}

					<!-- Header details -->
					<div class="receipt-header">
						<div>
							<h2 class="receipt-headline">{tape.headline}</h2>
							<p class="receipt-no" style:color={tale.theme}>
								#<span
									class="editable value-text"
									contenteditable
									bind:textContent={pen.no}
								></span>
							</p>
						</div>
						<div class="receipt-to-container">
							<h3
								class="receipt-to editable value-text"
								style:border-bottom-color={tale.theme}
								contenteditable
								bind:textContent={tale.to}
							></h3>
							<p
								class="receipt-address editable value-text"
								contenteditable
								bind:textContent={tale.toAddress}
							></p>
						</div>
					</div>

					<!-- Metadata -->
					<div class="receipt-meta">
						<div class="meta-row">
							<span class="meta-label">{tape.date}:</span>
							<span
								class="meta-val editable value-text"
								contenteditable
								bind:textContent={pen.date}
							></span>
						</div>
						<div class="meta-row">
							<span class="meta-label">{tape.from}:</span>
							<span
								class="meta-val editable value-text"
								contenteditable
								bind:textContent={pen.from}
							></span>
						</div>
					</div>

					<!-- Receipt details -->
					<div class="receipt-details">
						<div class="details-row">
							<span class="details-label">{tape.desc}:</span>
							<span
								class="details-val editable value-text"
								contenteditable
								bind:textContent={pen.desc}
							></span>
						</div>
						<div class="details-row">
							<span class="details-label">{tape.amount}:</span>
							<span
								class="details-val editable value-text"
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

					<!-- Summary details -->
					<div class="receipt-summary" style:border-top-color={tale.theme}>
						<div class="summary-row">
							<span class="summary-label"
								>{tape.tax} (<span
									class="editable value-text"
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
							<span class="summary-val value-text">{money(vat)}</span>
						</div>
						<div class="summary-row total" style:border-top-color={tale.theme}>
							<span class="total-label">{tape.total} (<span class="editable" contenteditable bind:textContent={tale.currency}></span>):</span>
							<span class="total-val value-text" style:color={tale.theme}>{money(total)}</span>
						</div>
					</div>

					<!-- Sign off / Thank you -->
					<div class="receipt-thankyou">
						<p class="signature-label">{tape.sign}</p>
						<p class="thankyou-msg" style:color={tale.theme} style:border-top-color={tale.theme}>
							{tape.thank}
						</p>
					</div>
				</div>
			{/each}

			<!-- Add Receipt Button -->
			<div class="add-receipt-container print-hidden">
				<button
					class="btn-add"
					title="Add another receipt"
					onclick={() => pens.push({ ...penStruct, no: (pens.length + 1).toString() })}
				>
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
	</main>
</div>
