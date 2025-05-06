let financeData = JSON.parse(localStorage.getItem("financeData")) || [];

function saveData() {
    const data = {
        month: new Date().toLocaleDateString('default', { year: 'numeric', month: 'short' }),
        salary: parseFloat(document.getElementById("salary").value) || 0,
        otherIncome: parseFloat(document.getElementById("other-income").value) || 0,
        fuelCost: parseFloat(document.getElementById("fuel-cost").value) || 0,
        shopping: parseFloat(document.getElementById("shopping").value) || 0,
        houseRent: parseFloat(document.getElementById("house-rent").value) || 0,
        otherCosts: parseFloat(document.getElementById("other-costs").value) || 0,
        _401k: parseFloat(document.getElementById("401k").value) || 0,
        stockInvestments: parseFloat(document.getElementById("stock-investments").value) || 0,
        hsa: parseFloat(document.getElementById("hsa").value) || 0,
        extraSavings: parseFloat(document.getElementById("extra-savings").value) || 0,
    };

    data.totalIncome = data.salary + data.otherIncome;
    data.totalExpenses = data.fuelCost + data.shopping + data.houseRent + data.otherCosts;
    data.totalInvestments = data._401k + data.stockInvestments + data.hsa + data.extraSavings;
    data.netBalance = data.totalIncome - data.totalExpenses;

    financeData.push(data);
    localStorage.setItem("financeData", JSON.stringify(financeData));

    document.getElementById("net-balance").innerText = `$${data.netBalance.toFixed(2)}`;
    renderCharts();
}

function renderCharts() {
    if (financeData.length === 0) return;
    const latest = financeData[financeData.length - 1];

    // Bar Chart
    const barCtx = document.getElementById("income-expense-chart").getContext("2d");
    if (window.barChart) window.barChart.destroy();
    window.barChart = new Chart(barCtx, {
        type: "bar",
        data: {
            labels: ["Income", "Expenses", "Net Balance"],
            datasets: [{
                label: "Amount ($)",
                data: [latest.totalIncome, latest.totalExpenses, latest.netBalance],
                backgroundColor: ["#4CAF50", "#F44336", "#2196F3"]
            }]
        }
    });

    // Pie Chart
    const pieCtx = document.getElementById("expense-breakdown-chart").getContext("2d");
    if (window.pieChart) window.pieChart.destroy();
    window.pieChart = new Chart(pieCtx, {
        type: "pie",
        data: {
            labels: ["Fuel", "Shopping", "Rent", "Other"],
            datasets: [{
                data: [latest.fuelCost, latest.shopping, latest.houseRent, latest.otherCosts],
                backgroundColor: ["#FF9800", "#9C27B0", "#03A9F4", "#E91E63"]
            }]
        }
    });

    // Line Chart
    const lineCtx = document.getElementById("investment-tracking-chart").getContext("2d");
    if (window.lineChart) window.lineChart.destroy();
    window.lineChart = new Chart(lineCtx, {
        type: "line",
        data: {
            labels: financeData.map(d => d.month),
            datasets: [
                {
                    label: "401k",
                    data: financeData.map(d => d._401k),
                    borderColor: "#4CAF50",
                    fill: false
                },
                {
                    label: "Stocks",
                    data: financeData.map(d => d.stockInvestments),
                    borderColor: "#FF9800",
                    fill: false
                },
                {
                    label: "HSA",
                    data: financeData.map(d => d.hsa),
                    borderColor: "#2196F3",
                    fill: false
                },
                {
                    label: "Extra Savings",
                    data: financeData.map(d => d.extraSavings),
                    borderColor: "#9C27B0",
                    fill: false
                }
            ]
        }
    });
}

function exportData() {
    if (financeData.length === 0) {
        alert("No data to export!");
        return;
    }

    const headers = Object.keys(financeData[0]).join(",");
    const rows = financeData.map(entry => Object.values(entry).join(","));
    const csv = [headers, ...rows].join("\n");

    const blob = new Blob([csv], { type: "text/csv;charset=utf-8;" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = "finance_data.csv";
    link.click();
}

window.onload = renderCharts;
