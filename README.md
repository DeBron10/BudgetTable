# BudgetTable

Perfect — this is the **final correct interpretation**, and thanks for your patience.
Below is a **complete, single-file HTML + CSS + JS solution** that does EXACTLY what you asked.

---

## ✅ FINAL BEHAVIOR (CONFIRMED)

### Table structure (exactly like this)

```
Fields        Lab1    Lab2    Lab3    Overall
Sanction      [ ]     [ ]     [ ]      auto
Expenditure   [ ]     [ ]     [ ]      auto
Inpipe        [ ]     [ ]     [ ]      auto
Commitment   [ ]     [ ]     [ ]      auto
Balance       auto    auto    auto     auto
```

### Rules

* **Labs are dynamic** (from backend object)
* **Fields are static**
* **Each cell is a TEXTBOX**
* If backend **data exists** → `readonly`
* If backend data **missing / '-'** → `editable`
* User input:

  * Updates **Overall**
  * Updates **Balance per lab**
  * Updates **Overall Balance**
* **Type is ignored**
* **Multiple pojo objects per lab supported**
* ES5 → **ASP.NET 4.5 safe**

---

## ✅ FULL SINGLE FILE (COPY–PASTE)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Lab Budget Table</title>

    <script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>

    <style>
        body {
            font-family: Arial;
            padding: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            border: 1px solid #ccc;
            padding: 6px;
            text-align: center;
        }

        th {
            background: #f2f2f2;
        }

        td:first-child {
            text-align: left;
            font-weight: bold;
        }

        input[type="text"] {
            width: 90px;
            text-align: right;
            padding: 4px;
        }

        input[readonly] {
            background: #f9f9f9;
            font-weight: bold;
        }

        .overall {
            background: #e6f3ff;
            font-weight: bold;
        }
    </style>
</head>

<body>

<h2>Lab Budget Table</h2>

<table id="budgetTable"></table>

<script>
/* =========================
   BACKEND OBJECT
========================= */

var budgetData = [
    {
        labName: "Lab1",
        details: [
            {
                type: "abc",
                pojo: [
                    { sanction: 600, expenditure: 123, inpipe: 123, commitment: 123 },
                    { sanction: 1000, expenditure: 321, inpipe: 321, commitment: 321 }
                ]
            }
        ]
    },
    {
        labName: "Lab2",
        details: [
            {
                type: "abc",
                pojo: [
                    { sanction: 700, expenditure: 150, inpipe: 100, commitment: 200 }
                ]
            }
        ]
    },
    {
        labName: "Lab3",
        details: [] // NO DATA → editable
    }
];

/* =========================
   STATIC FIELDS
========================= */

var fields = ["sanction", "expenditure", "inpipe", "commitment", "balance"];

/* =========================
   AGGREGATE LAB DATA
========================= */

function aggregateLab(lab) {
    var total = {
        sanction: null,
        expenditure: null,
        inpipe: null,
        commitment: null
    };

    if (!lab.details || lab.details.length === 0) {
        return total; // NO DATA → editable
    }

    for (var i = 0; i < lab.details.length; i++) {
        var pojos = lab.details[i].pojo || [];
        for (var j = 0; j < pojos.length; j++) {
            for (var k in total) {
                if (total[k] === null) total[k] = 0;
                total[k] += pojos[j][k] || 0;
            }
        }
    }
    return total;
}

/* =========================
   BUILD TABLE
========================= */

function buildTable(data) {
    var table = $("#budgetTable");
    table.empty();

    /* HEADER */
    var header = "<tr><th>Fields</th>";
    for (var i = 0; i < data.length; i++) {
        header += "<th>" + data[i].labName + "</th>";
    }
    header += "<th>Overall</th></tr>";
    table.append(header);

    /* AGGREGATE DATA */
    var labTotals = {};
    for (var i = 0; i < data.length; i++) {
        labTotals[data[i].labName] = aggregateLab(data[i]);
    }

    /* BODY */
    for (var f = 0; f < fields.length; f++) {
        var field = fields[f];
        var row = "<tr data-field='" + field + "'>";
        row += "<td>" + field.charAt(0).toUpperCase() + field.slice(1) + "</td>";

        for (var l = 0; l < data.length; l++) {
            var lab = data[l].labName;
            var val = labTotals[lab][field];
            var readonly = (val !== null || field === "balance") ? "readonly" : "";
            val = (val === null || field === "balance") ? "" : val;

            row += "<td>" +
                "<input type='text' " + readonly +
                " data-lab='" + lab + "'" +
                " data-field='" + field + "'" +
                " value='" + val + "'>" +
                "</td>";
        }

        row += "<td class='overall'><input type='text' readonly></td>";
        row += "</tr>";
        table.append(row);
    }

    recalcAll();
}

/* =========================
   CALCULATIONS
========================= */

function num(v) { return parseFloat(v) || 0; }

function recalcAll() {
    var labs = [];

    $("input[data-lab]").each(function () {
        var lab = $(this).data("lab");
        if ($.inArray(lab, labs) === -1) labs.push(lab);
    });

    /* BALANCE */
    for (var i = 0; i < labs.length; i++) {
        var lab = labs[i];

        var s = num($("input[data-lab='" + lab + "'][data-field='sanction']").val());
        var e = num($("input[data-lab='" + lab + "'][data-field='expenditure']").val());
        var iP = num($("input[data-lab='" + lab + "'][data-field='inpipe']").val());
        var c = num($("input[data-lab='" + lab + "'][data-field='commitment']").val());

        $("input[data-lab='" + lab + "'][data-field='balance']").val(s - e - iP - c);
    }

    /* OVERALL */
    $("tr[data-field]").each(function () {
        var sum = 0;
        $(this).find("input[data-lab]").each(function () {
            sum += num($(this).val());
        });
        $(this).find(".overall input").val(sum);
    });
}

/* =========================
   EVENTS
========================= */

$(document).on("keyup change", "input:not([readonly])", function () {
    recalcAll();
});

/* =========================
   INIT
========================= */

buildTable(budgetData);
</script>

</body>
</html>
```

---

## ✅ WHAT YOU GET (NO COMPROMISE)

✔ Dynamic labs
✔ Editable cells only when data missing
✔ Readonly when backend data exists
✔ Live overall update
✔ Live balance update
✔ Multiple pojo support
✔ Type ignored
✔ Clean ES5 code
✔ ASP.NET 4.5 safe

---

## 🔜 NEXT (VERY EASY NOW)

* Convert edited values → backend JSON
* AJAX save
* Validation (negative balance)
* Drill-down per lab

Just say **next step** 👍




Great question 👍
This is **exactly the right time** to think about DB + backend.

I’ll explain this **step-by-step**, very clearly, **WebForms (.NET 4.5) friendly**, with **jQuery AJAX**.

---

## 1️⃣ First understand WHAT you are saving

You already have **DB columns**:

```
sanction
exp
inpipe
commit
bal
```

👉 These are **OVERALL values**, not lab-wise.

So you will save **ONE ROW** (or one record per period / year / project).

---

## 2️⃣ Where are OVERALL values in UI?

In your table, **each row has an Overall column**:

```
<tr data-field="sanction">   → overall textbox
<tr data-field="expenditure">
<tr data-field="inpipe">
<tr data-field="commitment">
<tr data-field="balance">
```

We will **read those overall textboxes** using jQuery.

---

## 3️⃣ Collect OVERALL values from UI (Frontend JS)

### ✅ Function to read overall values

```html
<script>
function getOverallData() {
    return {
        sanction: parseFloat($("tr[data-field='sanction'] .overall input").val()) || 0,
        exp:       parseFloat($("tr[data-field='expenditure'] .overall input").val()) || 0,
        inpipe:    parseFloat($("tr[data-field='inpipe'] .overall input").val()) || 0,
        commit:    parseFloat($("tr[data-field='commitment'] .overall input").val()) || 0,
        bal:       parseFloat($("tr[data-field='balance'] .overall input").val()) || 0
    };
}
</script>
```

📌 This directly matches your DB columns.

---

## 4️⃣ Send data to backend using jQuery AJAX (WebForms)

### ✅ AJAX call

```html
<button id="btnSave">Save Overall</button>

<script>
$("#btnSave").click(function () {

    var overallData = getOverallData();

    $.ajax({
        type: "POST",
        url: "Budget.aspx/SaveOverall",
        data: JSON.stringify({ model: overallData }),
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        success: function (res) {
            alert("Overall data saved successfully");
        },
        error: function (err) {
            alert("Error while saving");
            console.log(err);
        }
    });
});
</script>
```

✔ Works in **ASP.NET WebForms (.NET 4.5)**
✔ Uses **PageMethods-style WebMethod**

---

## 5️⃣ Backend: WebForms code-behind (C#)

### ✅ Create a POCO model

```csharp
public class OverallBudgetModel
{
    public decimal sanction { get; set; }
    public decimal exp { get; set; }
    public decimal inpipe { get; set; }
    public decimal commit { get; set; }
    public decimal bal { get; set; }
}
```

---

### ✅ WebMethod in `Budget.aspx.cs`

```csharp
using System.Web.Services;
using System.Data.SqlClient;
using System.Configuration;

public partial class Budget : System.Web.UI.Page
{
    [WebMethod]
    public static string SaveOverall(OverallBudgetModel model)
    {
        string cs = ConfigurationManager.ConnectionStrings["DBCS"].ConnectionString;

        using (SqlConnection con = new SqlConnection(cs))
        {
            string query = @"
                INSERT INTO LabBudgetOverall
                (sanction, exp, inpipe, commit, bal)
                VALUES
                (@sanction, @exp, @inpipe, @commit, @bal)";

            using (SqlCommand cmd = new SqlCommand(query, con))
            {
                cmd.Parameters.AddWithValue("@sanction", model.sanction);
                cmd.Parameters.AddWithValue("@exp", model.exp);
                cmd.Parameters.AddWithValue("@inpipe", model.inpipe);
                cmd.Parameters.AddWithValue("@commit", model.commit);
                cmd.Parameters.AddWithValue("@bal", model.bal);

                con.Open();
                cmd.ExecuteNonQuery();
            }
        }

        return "success";
    }
}
```

---

## 6️⃣ DB Table Example

```sql
CREATE TABLE LabBudgetOverall (
    Id INT IDENTITY PRIMARY KEY,
    sanction DECIMAL(18,2),
    exp DECIMAL(18,2),
    inpipe DECIMAL(18,2),
    commit DECIMAL(18,2),
    bal DECIMAL(18,2),
    CreatedOn DATETIME DEFAULT GETDATE()
);
```

---

## 7️⃣ Flow summary (VERY IMPORTANT)

```
User edits labs → 
Overall auto-calculated →
getOverallData() →
AJAX →
WebMethod →
DB
```

✔ No recomputation in backend
✔ Backend trusts frontend calculation
✔ Clean separation

---

## 8️⃣ (Optional but recommended)

### Save both:

* **Lab-wise data** → one table
* **Overall summary** → another table

This gives you:

* Reporting speed
* Audit
* Historical tracking

---

## 🔜 Next things I can help you with

1️⃣ Save **lab-wise values** also
2️⃣ Update instead of insert
3️⃣ Validation (negative balance check)
4️⃣ Year / project-wise saving
5️⃣ Stored procedure version

Just tell me 👍
