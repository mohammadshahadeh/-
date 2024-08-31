<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>عقد تضمين شاشات الهاتف</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            width: 210mm; /* A4 width */
            height: 297mm; /* A4 height */
            padding: 20px;
            box-sizing: border-box;
            background-color: #f4f4f4;
            position: relative;
        }
        .header, .footer {
            text-align: center;
            color: #333;
        }
        .header {
            margin-bottom: 20px;
            position: relative;
        }
        .footer {
            margin-top: 20px;
            font-size: 0.9em;
            color: #666;
        }
        .company-name {
            font-size: 1.8em;
            font-weight: bold;
            color: #004085;
        }
        .current-date {
            margin-bottom: 10px;
            font-size: 1.1em;
        }
        .logo {
            position: absolute;
            top: -10px; /* رفع الصورة للأعلى أكثر */
            right: 20px;
            width: 100px;
            height: auto;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background-color: #fff;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 12px;
            text-align: center;
        }
        th {
            background-color: #e9ecef;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        .form-group input[type="text"], .form-group input[type="number"] {
            width: 100%;
            padding: 8px;
            font-size: 1em;
            box-sizing: border-box;
        }
        .btn, .print-btn {
            padding: 10px 15px;
            font-size: 1em;
            color: #fff;
            background-color: #007bff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
            display: inline-block;
        }
        .btn:hover, .print-btn:hover {
            background-color: #0056b3;
        }
        .print-btn {
            position: fixed; /* جعل الزر ثابت في الأسفل */
            bottom: 20px; /* المسافة من أسفل الصفحة */
            left: 20px; /* المسافة من اليسار */
            z-index: 1000; /* التأكد من ظهور الزر فوق المحتوى */
        }
        .hidden-print {
            display: none;
        }
        @media print {
            .hidden-print {
                display: none;
            }
            .print-only {
                display: block;
            }
            .header .logo {
                display: none;
            }
            .print-btn {
                display: none; /* إخفاء الزر عند الطباعة */
            }
        }
        .legal {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            background-color: #f8f9fa;
            font-size: 0.9em;
            line-height: 1.4;
        }
        .total-amount {
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 20px;
            text-align: right;
        }
        .price-symbol {
            font-size: 1em;
            margin-left: 5px;
        }
        .number-only {
            font-family: "Arial", sans-serif;
            direction: ltr;
        }
        input[type="number"] {
            -moz-appearance: textfield; /* إخفاء الأسهم في المتصفح */
        }
        input[type="number"]::-webkit-inner-spin-button, 
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="company-name">Shehadeh Center</div>
        <div class="current-date" id="current-date"></div>
        <img src="C:\Users\Hp\Desktop\453621997_497390449442169_2035267066132171401_n.jpg" alt="شعار الشركة" class="logo" />
    </div>

    <div class="form-group">
        <label for="person-name">اسم الشخص:</label>
        <input type="text" id="person-name" placeholder="ادخل اسم الشخص" />
    </div>

    <table>
        <thead>
            <tr>
                <th>رقم الصنف</th>
                <th>اسم الصنف</th>
                <th>الكمية</th>
                <th>السعر</th>
            </tr>
        </thead>
        <tbody id="product-table-body">
            <!-- صف فارغ مبدئي -->
            <tr>
                <td class="item-number">1</td>
                <td><input type="text" class="product-name" /></td>
                <td><input type="number" class="product-quantity number-only" /></td>
                <td>
                    <input type="text" class="product-price" placeholder="0" onkeydown="handleEnter(event, this)" />
                    <span class="price-symbol">₪</span>
                </td>
            </tr>
        </tbody>
    </table>

    <div class="total-amount">
        الإجمالي: <input type="text" id="total-amount" value="0₪" readonly />
    </div>

    <div class="legal">
        <h3>القوانين والشروط:</h3>
        <ul>
            <li>يجب على العملاء التحقق من تفاصيل الشاشات قبل الشراء.</li>
            <li>تخضع الأسعار للتغيير دون إشعار مسبق.</li>
            <li>الضمانات والشروط الخاصة تتوفر عند الطلب.</li>
            <li>لا تتحمل الشركة أي مسؤولية عن الأضرار الناتجة عن الاستخدام غير الصحيح.</li>
        </ul>
    </div>

    <div class="footer">
        <p>&copy; 2024 Shehadeh Center. جميع الحقوق محفوظة.</p>
    </div>

    <button class="print-btn" onclick="printDocument()">طباعة</button>

    <script>
        function updateDateTime() {
            const now = new Date();
            const options = { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long', hour: '2-digit', minute: '2-digit', second: '2-digit' };
            document.getElementById('current-date').innerText = now.toLocaleDateString('ar-EG', options);
        }

        function printDocument() {
            window.print();
        }

        function handleEnter(event, element) {
            if (event.key === 'Enter') {
                event.preventDefault(); // منع إدخال القيمة عند الضغط على Enter
                addRow();
            }
        }

        function addRow() {
            const tbody = document.getElementById('product-table-body');
            const rowCount = tbody.rows.length;
            const newRow = tbody.insertRow();
            newRow.innerHTML = `
                <td class="item-number">${rowCount + 1}</td>
                <td><input type="text" class="product-name" /></td>
                <td><input type="number" class="product-quantity number-only" /></td>
                <td>
                    <input type="text" class="product-price" placeholder="0" onkeydown="handleEnter(event, this)" />
                    <span class="price-symbol">₪</span>
                </td>
            `;
            updateItemNumbers();
        }

        function updateItemNumbers() {
            const rows = document.querySelectorAll('#product-table-body .item-number');
            rows.forEach((cell, index) => {
                cell.textContent = index + 1;
            });
        }

        function calculateTotal() {
            const rows = document.querySelectorAll('#product-table-body tr');
            let total = 0;
            rows.forEach(row => {
                const quantity = parseFloat(row.querySelector('.product-quantity').value) || 0;
                const price = parseFloat(row.querySelector('.product-price').value.replace('₪', '')) || 0;
                total += quantity * price;
            });
            document.getElementById('total-amount').value = `${total.toFixed(2)}₪`;
        }

        updateDateTime();
        setInterval(updateDateTime, 1000); // تحديث الوقت كل ثانية
        window.addEventListener('load', () => {
            const priceInputs = document.querySelectorAll('.product-price');
            priceInputs.forEach(input => {
                input.addEventListener('input', calculateTotal);
            });
        });
    </script>
</body>
</html>
