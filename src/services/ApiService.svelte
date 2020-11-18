<script context="module">
    export async function getExchangeData() {

        let usdData = [];

        let now = new Date();

        for (let i = 0; i < 30; i++) {
            let period = new Date(now.getFullYear(), now.getMonth()+1, now.getDate() - i);

            // it takes format yyyymmdd, example 20201118
            const apiUrl = `https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?date=${period.getFullYear()}${period.getMonth()}${period.getDate()}&json`;

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