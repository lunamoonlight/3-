// Расчет стоимости заказа
document.addEventListener('DOMContentLoaded', function () {
    const form = document.getElementById('calcForm');
    if (!form) return; // если формы нет на странице — выходим

    const productSelect = document.getElementById('product');
    const quantityInput = document.getElementById('quantity');
    const discountInput = document.getElementById('discount');
    const resultDiv = document.getElementById('result');
    const calcBtn = document.getElementById('calcBtn');

    calcBtn.addEventListener('click', function () {
    // Получаем значения
    const price = parseFloat(productSelect.value);
    const quantityRaw = quantityInput.value.trim();
    const discountRaw = discountInput.value.trim();

    // Проверка: количество не пустое и является числом
    if (quantityRaw === '') {
        resultDiv.textContent = 'Пожалуйста, укажите количество упаковок.';
        resultDiv.classList.add('show');
        return;
    }

    const quantity = Number(quantityRaw);

    // Проверка: целое ли число
    if (!Number.isInteger(quantity)) {
        resultDiv.textContent = 'Количество должно быть целым числом.';
        resultDiv.classList.add('show');
        return;
    }

    // Проверка: не отрицательное и не больше 200 000
    if (quantity < 0) {
        resultDiv.textContent = 'Количество не может быть отрицательным.';
        resultDiv.classList.add('show');
        return;
    }

    if (quantity > 200000) {
        resultDiv.textContent = 'Количество не должно превышать 200 000 упаковок.';
        resultDiv.classList.add('show');
        return;
    }

    // Проверка скидки
    const discount = discountRaw === '' ? 0 : Number(discountRaw);

    if (!Number.isFinite(discount)) {
        resultDiv.textContent = 'Скидка должна быть числом.';
        resultDiv.classList.add('show');
        return;
    }

    if (!Number.isInteger(discount)) {
        resultDiv.textContent = 'Скидка должна быть целым числом.';
        resultDiv.classList.add('show');
        return;
    }

    if (discount < 0) {
        resultDiv.textContent = 'Скидка не может быть отрицательной.';
        resultDiv.classList.add('show');
        return;
    }

    if (discount > 100) {
        resultDiv.textContent = 'Скидка не может превышать 100%.';
        resultDiv.classList.add('show');
        return;
    }

    // Если всё корректно — считаем
    let total = price * quantity;
    if (discount > 0) {
        total = total * (1 - discount / 100);
    }

    const formattedTotal = total.toFixed(2);

    resultDiv.innerHTML = `Итоговая стоимость: <strong>${formattedTotal} ₽</strong>`;
    resultDiv.classList.add('show');

    // Анимация подсветки
    resultDiv.style.transform = 'scale(0.98)';
    setTimeout(() => {
        resultDiv.style.transform = 'scale(1)';
    }, 150);
});

    // Автоматический пересчёт при изменении полей (опционально)
    [productSelect, quantityInput, discountInput].forEach(el => {
        el.addEventListener('change', function () {
            if (resultDiv.classList.contains('show')) {
                calcBtn.click(); // пересчитываем, если результат уже показан
            }
        });
    });
});

// Анимация меню
window.addEventListener('load', function () {
    const navLinks = document.querySelectorAll('.nav-menu > li > a');
    navLinks.forEach((link, index) => {
        link.style.opacity = '0';
        link.style.transform = 'translateY(-5px)';
        setTimeout(() => {
            link.style.transition = 'opacity 0.4s ease, transform 0.4s ease';
            link.style.opacity = '1';
            link.style.transform = 'translateY(0)';
        }, 100 * index);
    });
});