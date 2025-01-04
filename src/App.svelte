<script>
  let pen = $state({
    no: '20231027-001',
    date: 'October 27, 2023',
    from: 'Jane Doe',
    to: 'AC',
    desc: 'Consultation Services',
    amount: 150.00,
    vat: 0,
  })
</script>

<div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md mt-10 border-t-8 border-brand-blue">
  <div class="flex justify-between items-center mb-4">
    <div>
      <h2 class="text-xl font-bold text-gray-800">Cash Receipt</h2>
      <p class="text-sm text-brand-blue">#<span>{pen.no}</span></p>
    </div>
    <div>
      <div class="w-12 h-12 rounded-full bg-brand-blue text-white flex items-center justify-center font-bold text-xl">
        {pen.to}
      </div>
    </div>
  </div>

  <div class="mb-4 text-sm">
    <div class="flex justify-between">
      <span class="text-gray-600">Date:</span>
      <span class="text-gray-700">{pen.date}</span>
    </div>
    <div class="flex justify-between">
      <span class="text-gray-600">Received From:</span>
      <span class="text-gray-700">{pen.from}</span>
    </div>
  </div>

  <div class="border-t border-b border-gray-200 py-3 my-3 text-sm">
    <div class="flex justify-between mb-1">
      <span class="text-gray-600">Description:</span>
      <span class="text-gray-700">{pen.desc}</span>
    </div>
    <div class="flex justify-between">
      <span class="text-gray-600">Amount:</span>
      <span class="text-gray-700">{pen.amount}</span>
    </div>
  </div>

  <div class="text-sm font-medium">
    <div class="flex justify-between">
      <span class="text-gray-700">Tax (0%):</span>
      <span class="text-gray-700">$0.00</span>
    </div>
    <div class="flex justify-between font-bold text-lg mt-2">
      <span class="text-gray-800">Total:</span>
      <span class="text-gray-800">$150.00</span>
    </div>
  </div>

  <div class="mt-6 text-center text-xs text-gray-500">
    <p>Thank you for your business!</p>
    <p>123 Business Ave, Anytown</p>
  </div>
</div>