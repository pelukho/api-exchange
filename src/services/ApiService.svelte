<script context="module">
    export async function getExchangeData() {

        let usdData = [];

        let now = new Date();

        for (let i = 0; i < 30; i++) {
            let period = new Date(now.getFullYear(), now.getMonth(), now.getDate() - i),
                year = period.getFullYear(),
                month = period.getMonth() + 1,
                day = period.getDate();

            month = month < 10 ? '0' + month : month;

            // it takes format yyyymmdd, example 20201118
            const apiUrl = `https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?date=${year}${month}${day}&json`;

            const response = await fetch(`${apiUrl}`, {
                method: 'GET',
                mode: 'cors',
                cache: 'no-cache',
                credentials: 'same-origin',
                redirect: 'follow',
                referrerPolicy: 'no-referrer'
            });

            let data = await response.json();
            usdData.push((data.filter(item => item.cc === 'USD'))[0]);
        }

        return usdData;
    }
</script>