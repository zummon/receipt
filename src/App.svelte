<script>
	import { onMount } from "svelte";

	const bund = {
		en: {
			family: "'Plus Jakarta Sans', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Courier+Prime:wght@400;700&display=swap",
			headline: "Receipt",
			date: "Date",
			from: "Received From",
			desc: "Description",
			amount: "Amount",
			tax: "Tax",
			total: "Total",
			thank: "Thank you for your business!",
			sign: "Signature",
			totalDocs: "Total Documents",
			importTitle: "Import Document Data",
			guideTitle: "TSV Data Structure Guide",
			guideDesc: "Your file must start with these exact column headers in the first row, separated by tabs:",
			guideHeader: "Header Name",
			guideType: "Type",
			guideExample: "Example",
			guideTypeText: "Text",
			guideTypeNum: "Number",
			uploadAction: "Click to upload a .tsv file",
			uploadOrDrag: "or drag and drop here",
			uploadHint: "TSV (Tab-Separated Values) files only",
			pasteLabel: "Or paste raw tab-separated (TSV) values below:",
			closeLabel: "Close",
		},
		th: {
			family: "'Sarabun', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap",
			headline: "ใบเสร็จรับเงิน",
			date: "วันที่",
			from: "รับเงินจาก",
			desc: "รายละเอียด",
			amount: "จำนวน",
			tax: "ภาษี",
			total: "รวมทั้งสิ้น",
			thank: "ขอบคุณสำหรับการอุดหนุน!",
			sign: "ลายเซ็นผู้รับเงิน",
			totalDocs: "เอกสารทั้งหมด",
			importTitle: "นำเข้าข้อมูลเอกสาร",
			guideTitle: "คู่มือโครงสร้างข้อมูล TSV",
			guideDesc: "ไฟล์ของคุณต้องเริ่มต้นด้วยหัวข้อคอลัมน์เหล่านี้ในแถวแรก โดยคั่นด้วยแท็บ (Tab):",
			guideHeader: "ชื่อหัวข้อ",
			guideType: "ประเภท",
			guideExample: "ตัวอย่าง",
			guideTypeText: "ข้อความ",
			guideTypeNum: "ตัวเลข",
			uploadAction: "คลิกเพื่ออัปโหลดไฟล์ .tsv",
			uploadOrDrag: "หรือลากไฟล์มาวางที่นี่",
			uploadHint: "เฉพาะไฟล์ TSV (ค่าคั่นด้วยแท็บ) เท่านั้น",
			pasteLabel: "หรือวางค่าคั่นด้วยแท็บ (TSV) ด้านล่าง:",
			closeLabel: "ปิด",
		},
		es: {
			family: "'Plus Jakarta Sans', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Courier+Prime:wght@400;700&display=swap",
			headline: "Recibo",
			date: "Fecha",
			from: "Recibido De",
			desc: "Descripción",
			amount: "Cantidad",
			tax: "Impuesto",
			total: "Total",
			thank: "¡Gracias por su compra!",
			sign: "Firma",
			totalDocs: "Documentos Totales",
			importTitle: "Importar Datos del Documento",
			guideTitle: "Guía de Estructura de Datos TSV",
			guideDesc: "Su archivo debe comenzar con estos encabezados de columna en la primera fila, separados por pestañas (tabs):",
			guideHeader: "Nombre del Encabezado",
			guideType: "Tipo",
			guideExample: "Ejemplo",
			guideTypeText: "Texto",
			guideTypeNum: "Número",
			uploadAction: "Haga clic para cargar un archivo .tsv",
			uploadOrDrag: "o arrastre y suelte aquí",
			uploadHint: "Solo archivos TSV (valores separados por tabulaciones)",
			pasteLabel: "O pegue valores separados por tabulaciones (TSV) abajo:",
			closeLabel: "Cerrar",
		},
		fr: {
			family: "'Plus Jakarta Sans', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Courier+Prime:wght@400;700&display=swap",
			headline: "Reçu",
			date: "Date",
			from: "Reçu De",
			desc: "Description",
			amount: "Montant",
			tax: "Taxe",
			total: "Total",
			thank: "Merci pour votre achat !",
			sign: "Signature",
			totalDocs: "Total Documents",
			importTitle: "Importer les Données",
			guideTitle: "Guide de Structure des Données TSV",
			guideDesc: "Votre fichier doit commencer par ces en-têtes exacts dans la première ligne, séparés par des tabulations:",
			guideHeader: "Nom de l'En-tête",
			guideType: "Type",
			guideExample: "Exemple",
			guideTypeText: "Texte",
			guideTypeNum: "Nombre",
			uploadAction: "Cliquez pour charger un fichier .tsv",
			uploadOrDrag: "ou glissez-déposez ici",
			uploadHint: "Fichiers TSV (valeurs séparées par des tabulations) uniquement",
			pasteLabel: "Ou collez les valeurs séparées par des tabulations (TSV) ci-dessous :",
			closeLabel: "Fermer",
		},
		de: {
			family: "'Plus Jakarta Sans', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Courier+Prime:wght@400;700&display=swap",
			headline: "Quittung",
			date: "Datum",
			from: "Erhalten Von",
			desc: "Beschreibung",
			amount: "Betrag",
			tax: "Steuer",
			total: "Gesamt",
			thank: "Vielen Dank für Ihren Einkauf!",
			sign: "Unterschrift",
			totalDocs: "Dokumente Gesamt",
			importTitle: "Dokumentdaten Importieren",
			guideTitle: "TSV-Datenstruktur-Anleitung",
			guideDesc: "Ihre Datei muss in der ersten Zeile mit diesen Spaltenüberschriften beginnen, getrennt durch Tabulatoren:",
			guideHeader: "Spaltenname",
			guideType: "Typ",
			guideExample: "Beispiel",
			guideTypeText: "Text",
			guideTypeNum: "Nummer",
			uploadAction: "Klicken Sie zum Hochladen einer .tsv-Datei",
			uploadOrDrag: "oder ziehen Sie sie hierher",
			uploadHint: "Nur TSV-Dateien (tabulatorgetrennte Werte)",
			pasteLabel: "Oder fügen Sie tabulatorgetrennte (TSV) Werte unten ein:",
			closeLabel: "Schließen",
		},
		ja: {
			family: "'Noto Sans JP', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&display=swap",
			headline: "領収書",
			date: "日付",
			from: "受取人",
			desc: "説明",
			amount: "金額",
			tax: "税",
			total: "合計",
			thank: "ご利用ありがとうございます",
			sign: "署名",
			totalDocs: "合計件数",
			importTitle: "データをインポート",
			guideTitle: "TSVデータ構造ガイド",
			guideDesc: "ファイルは最初の行に以下の列ヘッダーを持ち、タブで区切られている必要があります。",
			guideHeader: "ヘッダー名",
			guideType: "データ型",
			guideExample: "例",
			guideTypeText: "テキスト",
			guideTypeNum: "数値",
			uploadAction: ".tsvファイルをアップロード",
			uploadOrDrag: "またはここにドラッグ＆ドロップ",
			uploadHint: "TSV（タブ区切り値）ファイルのみ",
			pasteLabel: "または以下にタブ区切り値（TSV）を貼り付けます：",
			closeLabel: "閉じる",
		},
		"zh-CN": {
			family: "'Noto Sans SC', sans-serif",
			fontUrl: "https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap",
			headline: "收据",
			date: "日期",
			from: "收款人",
			desc: "描述",
			amount: "金额",
			tax: "税",
			total: "总计",
			thank: "感谢您的惠顾！",
			sign: "签名",
			totalDocs: "单据总数",
			importTitle: "导入单据数据",
			guideTitle: "TSV 数据结构指南",
			guideDesc: "您的文件必须在第一行以这些精确的列标题开始，并用制表符（tab）分隔：",
			guideHeader: "标题名称",
			guideType: "类型",
			guideExample: "示例",
			guideTypeText: "文本",
			guideTypeNum: "数字",
			uploadAction: "点击上传 .tsv 文件",
			uploadOrDrag: "或拖放到此处",
			uploadHint: "仅限 TSV（制表符分隔值）文件",
			pasteLabel: "或在下方粘贴制表符分隔（TSV）的值：",
			closeLabel: "关闭",
		},
	};

	const penStruct = {
		no: "20231027-002",
		date: "October 27, 2023",
		from: "Mac Donel",
		desc: "Consultation Services",
		amount: 80.0,
	};

	const docBundles = {
		receipt: {
			en: { headline: "Receipt", from: "Received From" },
			th: { headline: "ใบเสร็จรับเงิน", from: "รับเงินจาก" },
			es: { headline: "Recibo", from: "Recibido de" },
			fr: { headline: "Reçu", from: "Reçu de" },
			de: { headline: "Quittung", from: "Erhalten von" },
			ja: { headline: "領収書", from: "領収先" },
			"zh-CN": { headline: "收据", from: "收款人" },
		},
		cash_receipt: {
			en: { headline: "Cash Receipt", from: "Received From" },
			th: { headline: "ใบเสร็จรับเงินสด", from: "รับเงินจาก" },
			es: { headline: "Recibo de Caja", from: "Recibido de" },
			fr: { headline: "Reçu de Caisse", from: "Reçu de" },
			de: { headline: "Barquittung", from: "Erhalten von" },
			ja: { headline: "現金領収書", from: "領収先" },
			"zh-CN": { headline: "现金收据", from: "收款人" },
		},
		invoice: {
			en: { headline: "Invoice", from: "Bill To" },
			th: { headline: "ใบแจ้งหนี้ / ใบกำกับภาษี", from: "ชื่อลูกค้า" },
			es: { headline: "Factura", from: "Facturar a" },
			fr: { headline: "Facture", from: "Facturer à" },
			de: { headline: "Rechnung", from: "Rechnungsempfänger" },
			ja: { headline: "請求書", from: "請求先" },
			"zh-CN": { headline: "发票", from: "账单寄送至" },
		},
		quotation: {
			en: { headline: "Quotation", from: "Quote To" },
			th: { headline: "ใบเสนอราคา", from: "ลูกค้า" },
			es: { headline: "Cotización", from: "Cotizar a" },
			fr: { headline: "Devis", from: "Devis pour" },
			de: { headline: "Angebot", from: "Kunde" },
			ja: { headline: "見積書", from: "見積先" },
			"zh-CN": { headline: "报价单", from: "客户" },
		},
		purchase_order: {
			en: { headline: "Purchase Order", from: "Vendor" },
			th: { headline: "ใบสั่งซื้อ", from: "ผู้ขาย" },
			es: { headline: "Orden de Compra", from: "Proveedor" },
			fr: { headline: "Bon de Commande", from: "Fournisseur" },
			de: { headline: "Bestellung", from: "Lieferant" },
			ja: { headline: "注文書", from: "発注先" },
			"zh-CN": { headline: "采购单", from: "供应商" },
		},
		delivery_order: {
			en: { headline: "Delivery Order", from: "Deliver To" },
			th: { headline: "ใบส่งสินค้า", from: "ส่งถึง" },
			es: { headline: "Orden de Entrega", from: "Entregar a" },
			fr: { headline: "Bon de Livraison", from: "Livrer à" },
			de: { headline: "Lieferschein", from: "Empfänger" },
			ja: { headline: "納品書", from: "納品先" },
			"zh-CN": { headline: "送货单", from: "送货至" },
		},
	};

	let store = $state({
		logo: "",
	});

	let tale = $state({
		theme: "#2d6a4f", // deep forest green default
		lang: "en",
		docType: "receipt",
		vat: 0.07,
		to: "My company",
		toAddress: "444 Globe, Anytown",
		currency: "$",
	});

	let pens = $state([{ ...penStruct }]);
	let savedUrl = $state("");
	let showDataInput = $state(false);
	let showToast = $state(false);
	let rawTsvText = $state("");

	function arrToStr(arr) {
		if (!arr || arr.length === 0) return "";
		const headers = ["no", "date", "from", "desc", "amount"];
		const headerLine = headers.join("\t");
		const lines = arr.map(item => headers.map(header => item[header] !== undefined ? item[header] : "").join("\t"));
		return [headerLine, ...lines].join("\n");
	}

	function openModal() {
		rawTsvText = arrToStr(pens);
		showDataInput = true;
	}

	function handleFileChange(e) {
		const file = e.target.files[0];
		if (file) {
			const reader = new FileReader();
			reader.onload = (event) => {
				const content = event.target.result;
				rawTsvText = content;
				strToArr(content);
			};
			reader.readAsText(file);
		}
	}

	function handleTextareaInput(e) {
		rawTsvText = e.target.value;
		strToArr(rawTsvText);
	}

	let tape = $derived({
		...bund.en,
		...bund[tale.lang],
		headline: docBundles[tale.docType]?.[tale.lang]?.headline || docBundles[tale.docType]?.en?.headline || "Document",
		from: docBundles[tale.docType]?.[tale.lang]?.from || docBundles[tale.docType]?.en?.from || "Received From",
	});

	let overallTotal = $derived(
		pens.reduce((sum, pen) => {
			const amt = Number(pen.amount) || 0;
			const vt = amt * Number(tale.vat);
			return sum + amt + vt;
		}, 0),
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
		const baseUrl = "https://codepen.io/zummon/full/ogvoyzN";
		savedUrl = baseUrl + "?" + pass.toString();
		navigator.clipboard.writeText(savedUrl);

		showToast = true;
		setTimeout(() => {
			showToast = false;
		}, 2500);
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
	<link
		href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&display=swap"
		rel="stylesheet"
	/>
	{#if tape.fontUrl}
		<link href={tape.fontUrl} rel="stylesheet" />
	{/if}
</svelte:head>

<div class="app-container">
	<!-- Sidebar (Dashboard) -->
	<aside class="sidebar print-hidden">
		<h2 class="sidebar-title">Dashboard</h2>

		<div class="sidebar-metric">
			<span class="sidebar-metric-label">{tape.totalDocs}</span>
			<span class="sidebar-metric-value">{pens.length}</span>
		</div>

		<div class="sidebar-metric">
			<span class="sidebar-metric-label">Overall Total</span>
			<span class="sidebar-metric-value" style:color={tale.theme}
				>{tale.currency || ""} {money(overallTotal)}</span
			>
		</div>
	</aside>

	<!-- Main Content Area -->
	<main class="main-content">
		<!-- Control Bar -->
		<div class="control-bar print-hidden">
			<div class="control-item">
				<button
					class="btn"
					title="Input data (TSV format)"
					onclick={openModal}
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
					<span>Data Input</span>
				</button>
			</div>

			<div class="control-item">
				<button
					class="btn"
					class:copied={showToast}
					title="Copy shareable link to clipboard"
					onclick={finish}
				>
					{#if showToast}
						<svg
							xmlns="http://www.w3.org/2000/svg"
							fill="none"
							viewBox="0 0 24 24"
							stroke-width="2.5"
							stroke="currentColor"
							style="color: #10b981;"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								d="M4.5 12.75l6 6 9-13.5"
							/>
						</svg>
						<span>Copied!</span>
					{:else}
						<svg
							xmlns="http://www.w3.org/2000/svg"
							fill="none"
							viewBox="0 0 24 24"
							stroke-width="2"
							stroke="currentColor"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								d="M13.19 8.688a4.5 4.5 0 011.242 7.244l-4.5 4.5a4.5 4.5 0 01-6.364-6.364l1.757-1.757m13.35-.622l1.757-1.757a4.5 4.5 0 00-6.364-6.364l-4.5 4.5a4.5 4.5 0 001.242 7.244"
							/>
						</svg>
						<span>Share Link</span>
					{/if}
				</button>
			</div>

			<div class="control-item">
				<button class="btn" title="Print receipts" onclick={() => print()}>
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
					<span>Print</span>
				</button>
			</div>

			<div class="control-item">
				<select
					class="select-input"
					title="Change document type"
					bind:value={tale.docType}
				>
					{#each Object.keys(docBundles) as key}
						<option value={key}>
							{docBundles[key][tale.lang]?.headline || docBundles[key].en.headline}
						</option>
					{/each}
				</select>
			</div>

			<div class="control-item">
				<select
					class="select-input"
					title="Change language"
					bind:value={tale.lang}
				>
					{#each Object.keys(bund) as value}
						<option {value}>{value.toUpperCase()}</option>
					{/each}
				</select>
			</div>

			<div class="control-item">
				<div
					class="color-picker-wrapper"
					style:background-color={tale.theme}
					title="Theme Color"
				>
					<input class="color-input" type="color" bind:value={tale.theme} />
				</div>
			</div>
		</div>

		<!-- TSV Import Modal Dialog -->
		{#if showDataInput}
			<div class="modal-overlay print-hidden" role="button" tabindex="-1" onclick={() => showDataInput = false} onkeydown={(e) => { if (e.key === 'Escape') showDataInput = false; }}>
				<div class="modal-card" role="presentation" onclick={(e) => e.stopPropagation()}>
					<div class="modal-header">
						<h3 class="modal-title">{tape.importTitle}</h3>
						<button class="modal-close" onclick={() => showDataInput = false} title={tape.closeLabel}>
							<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.5">
								<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
							</svg>
						</button>
					</div>
					<div class="modal-body">
						<!-- Structure Guide -->
						<div class="guide-box">
							<h4 class="guide-title">
								<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
									<path stroke-linecap="round" stroke-linejoin="round" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
								</svg>
								{tape.guideTitle}
							</h4>
							<p class="guide-desc">{tape.guideDesc}</p>
							<div class="guide-table-wrapper">
								<table class="guide-table">
									<thead>
										<tr>
											<th>{tape.guideHeader}</th>
											<th>{tape.guideType}</th>
											<th>{tape.guideExample}</th>
										</tr>
									</thead>
									<tbody>
										<tr>
											<td><code>no</code></td>
											<td>{tape.guideTypeText}</td>
											<td>20231027-002</td>
										</tr>
										<tr>
											<td><code>date</code></td>
											<td>{tape.guideTypeText}</td>
											<td>October 27, 2023</td>
										</tr>
										<tr>
											<td><code>from</code></td>
											<td>{tape.guideTypeText}</td>
											<td>Mac Donel</td>
										</tr>
										<tr>
											<td><code>desc</code></td>
											<td>{tape.guideTypeText}</td>
											<td>Consultation Services</td>
										</tr>
										<tr>
											<td><code>amount</code></td>
											<td>{tape.guideTypeNum}</td>
											<td>80.00</td>
										</tr>
									</tbody>
								</table>
							</div>
						</div>

						<!-- Drag & Drop / File Select Zone -->
						<div class="file-upload-zone">
							<svg class="upload-icon" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />
							</svg>
							<div class="upload-text">
								<span class="upload-action">{tape.uploadAction}</span> {tape.uploadOrDrag}
							</div>
							<div class="upload-hint">{tape.uploadHint}</div>
							<input type="file" accept=".tsv" class="file-input-hidden" onchange={handleFileChange} />
						</div>

						<!-- Copy & Paste Textarea -->
						<div class="textarea-section">
							<label class="textarea-label" for="tsv-paste-input">{tape.pasteLabel}</label>
							<textarea
								id="tsv-paste-input"
								class="modal-textarea"
								placeholder="no&#9;date&#9;from&#9;desc&#9;amount&#13;20231027-002&#9;October 27, 2023&#9;Mac Donel&#9;Consultation Services&#9;80.00"
								bind:value={rawTsvText}
								oninput={(e) => strToArr(e.currentTarget.value)}
							></textarea>
						</div>
					</div>
				</div>
			</div>
		{/if}

		<!-- Receipts Display Container -->
		<div
			class="receipts-container"
			style:font-family={tape.family}
		>
			{#each pens as pen, index (index)}
				{@const vat = pen.amount * Number(tale.vat)}
				{@const total = pen.amount + vat}
				<div class="receipt-card page-break" style:--theme-color={tale.theme}>
					<!-- Delete button -->
					<button
						class="btn-delete print-hidden"
						title="Delete"
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

					<!-- Watermarks removed for clean layout -->

					<!-- Logo Display / Upload -->
					<div class="logo-container">
						{#if store.logo}
							<label class="logo-wrapper" title="Change Logo">
								<input style="display: none;" type="file" onchange={upload} />
								<img class="logo-img" src={store.logo} alt="Logo" />
							</label>
						{:else}
							<label class="logo-placeholder print-hidden" title="Upload Logo">
								<input style="display: none;" type="file" onchange={upload} />
								<svg
									class="logo-placeholder-icon"
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
										d="M12 5v14m-7-7h14"
									/>
								</svg>
								<span>Logo</span>
							</label>
						{/if}
					</div>

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
							<span class="total-label"
								>{tape.total} (<span
									class="editable"
									contenteditable
									bind:textContent={tale.currency}
								></span>):</span
							>
							<span class="total-val value-text" style:color={tale.theme}
								>{money(total)}</span
							>
						</div>
					</div>

					<!-- Sign off / Thank you -->
					<div class="receipt-thankyou">
						<p class="signature-label">{tape.sign}</p>
						<p
							class="thankyou-msg"
							style:color={tale.theme}
							style:border-top-color={tale.theme}
						>
							{tape.thank}
						</p>
					</div>
				</div>
			{/each}

			<!-- Add Receipt Button -->
			<div class="add-receipt-container print-hidden">
				<button
					class="btn-add"
					title="Add document"
					onclick={() =>
						pens.push({ ...penStruct, no: (pens.length + 1).toString() })}
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
		<!-- Toast Notification -->
		{#if showToast}
			<div class="toast print-hidden">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="2.5"
					stroke="currentColor"
					class="toast-icon"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M4.5 12.75l6 6 9-13.5"
					/>
				</svg>
				<span>Link copied to clipboard!</span>
			</div>
		{/if}
	</main>
</div>
