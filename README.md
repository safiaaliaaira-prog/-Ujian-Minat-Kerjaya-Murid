let score = { s: 0, st: 0, t: 0 };
    let qList = [];

    function startQuiz() {
        qList = [...questions].sort(() => Math.random() - 0.5);
        current = 0;
        score = { s: 0, st: 0, t: 0 };
        document.getElementById('start-view').style.display = 'none';
        document.getElementById('question-view').style.display = 'block';
        document.getElementById('quiz-screen').style.display = 'block';
        document.getElementById('result-screen').style.display = 'none';
        showQuestion();
    }

    function showQuestion() {
        const q = qList[current];
        const p = Math.round((current / qList.length) * 100);
        document.getElementById('q-label').innerText = `Soalan ${current + 1}/${qList.length}`;
        document.getElementById('q-percent').innerText = p + "%";
        document.getElementById('progress-fill').style.width = p + "%";
        document.getElementById('question-text').innerText = q.q;
    }

    function answer(yes) {
        if(yes) score[qList[current].f]++;
        current++;
        if(current < qList.length) {
            showQuestion();
        } else {
            showFinalResult();
        }
    }

    function showFinalResult() {
        document.getElementById('quiz-screen').style.display = 'none';
        document.getElementById('result-screen').style.display = 'block';

        let top = Object.keys(score).reduce((a, b) => score[a] > score[b] ? a : b);
        const banner = document.getElementById('res-banner');
        const title = document.getElementById('res-title');
        const desc = document.getElementById('res-desc');

        if(top === 's') {
            banner.style.backgroundColor = 'var(--sains-bg)';
            title.innerText = "ALIRAN SAINS";
            desc.innerText = "Awak sangat logik dan analitikal! Awak sesuai dalam bidang perubatan, kejuruteraan atau teknologi.";
        } else if(top === 'st') {
            banner.style.backgroundColor = 'var(--sastera-bg)';
            title.innerText = "ALIRAN SASTERA";
            desc.innerText = "Awak berjiwa kreatif dan komunikatif! Bidang undang-undang, media atau seni sangat serasi dengan awak.";
        } else {
            banner.style.backgroundColor = 'var(--teknik-bg)';
            title.innerText = "ALIRAN TEKNIKAL";
            desc.innerText = "Awak pakar dalam kemahiran praktikal! Bidang vokasional, kulinari atau teknikal adalah kekuatan awak.";
        }

        document.getElementById('s-score').innerText = score.s;
        document.getElementById('st-score').innerText = score.st;
        document.getElementById('t-score').innerText = score.t;

        // Simpan rekod ke memori telefon/browser
        const hist = JSON.parse(localStorage.getItem('saf_quiz_history') || "[]");
        hist.unshift({ a: title.innerText, d: new Date().toLocaleDateString('ms-MY') });
        localStorage.setItem('saf_quiz_history', JSON.stringify(hist.slice(0, 5)));
        renderHistory();
    }

    function renderHistory() {
        const hist = JSON.parse(localStorage.getItem('saf_quiz_history') || "[]");
        const container = document.getElementById('history-container');
        if(hist.length === 0) {
            container.innerHTML = "<p style='color:#999;font-size:0.8rem;'>Tiada rekod.</p>";
            return;
        }
        container.innerHTML = hist.map(i => `
            <div class="history-item"><span><b>${i.a}</b></span><span>${i.d}</span></div>
        `).join('');
    }

    window.onload = renderHistory;
</script>

</body>
</html>
