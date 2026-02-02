<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2026 사각사각 - Paper Journal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        /* 폰트 추가: 감자꽃(Gamja Flower) 및 기존 보조 폰트들 */
        @import url('https://fonts.googleapis.com/css2?family=Gamja+Flower&family=Poor+Story&family=Gowon+Dodum&family=Noto+Serif+KR:wght@300;400;700&family=Nanum+Pen+Script&display=swap');
        
        :root {
            --paper-color: #fcfcfc;
            --line-color: #f0f0f0;
            --deep-pink: #ff8fa3;
            --star-yellow: #f1c40f;
            --text-main: #2d3436;
        }

        body {
            font-family: 'Noto Serif KR', serif;
            background-color: #f4f4f4;
            color: var(--text-main);
            line-height: 1.6;
            margin: 0;
            display: flex;
            justify-content: center;
        }

        .notebook {
            width: 100%;
            max-width: 800px;
            min-height: 100vh;
            background-color: var(--paper-color);
            background-image: linear-gradient(var(--line-color) 1px, transparent 1px);
            background-size: 100% 3rem;
            box-shadow: 0 0 30px rgba(0,0,0,0.08);
            position: relative;
            padding-bottom: 120px;
        }

        /* 감자꽃(Gamja Flower) 스타일 클래스 */
        .gamja-flower {
            font-family: 'Gamja Flower', cursive;
        }

        header {
            padding: 4.5rem 0 2.5rem;
            text-align: center;
        }

        header h1 {
            font-family: 'Gamja Flower', cursive;
            font-weight: 400;
            letter-spacing: 0.02em;
            font-size: 2.5rem;
            opacity: 0.9;
            color: var(--text-main);
            transition: all 0.3s ease;
        }

        #book-list {
            padding: 0 3.5rem;
            display: flex;
            flex-direction: column;
            gap: 2rem;
            min-height: 650px;
        }

        .book-tab {
            background: white;
            border: 1px solid #eee;
            border-radius: 12px; /* 폰트에 맞춰 조금 더 둥글게 조정 */
            padding: 1.5rem;
            display: flex;
            gap: 1.5rem;
            align-items: flex-start;
            position: relative;
            transition: all 0.3s ease;
            box-shadow: 2px 2px 8px rgba(0,0,0,0.02);
        }

        .book-tab:hover {
            transform: translateX(5px) translateY(-2px);
            box-shadow: 5px 8px 15px rgba(0,0,0,0.04);
        }

        .cover-slot {
            width: 80px;
            height: 110px;
            background-color: #fafafa;
            border: 1px solid #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            overflow: hidden;
            flex-shrink: 0;
            border-radius: 8px;
        }

        .cover-slot img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .star-rating i {
            font-size: 1rem;
            color: #f0f0f0;
            cursor: pointer;
            margin-right: 2px;
            transition: color 0.2s ease;
        }

        .star-rating i.active {
            color: var(--star-yellow);
        }

        .review-input {
            width: 100%;
            border: none;
            outline: none;
            background: transparent;
            resize: none;
            font-size: 1.4rem;
            color: #444;
            margin-top: 0.4rem;
            line-height: 1.4;
            height: auto;
            font-family: 'Gamja Flower', cursive;
        }

        .pagination-container {
            position: absolute;
            bottom: 2.5rem;
            left: 0;
            right: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 1rem;
            z-index: 10;
        }

        .pagination-wrapper {
            display: flex;
            align-items: center;
            gap: 1.5rem;
            background: rgba(255, 255, 255, 0.9);
            padding: 0.4rem 1.2rem;
            border-radius: 30px;
            border: 1px solid #f0f0f0;
        }

        .page-btn {
            font-size: 1.2rem;
            color: #aaa;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            user-select: none;
            font-family: 'Gamja Flower', cursive;
        }

        .page-btn:hover:not(.disabled) {
            color: var(--deep-pink);
        }

        .page-btn.disabled {
            opacity: 0.15;
            cursor: default;
            pointer-events: none;
        }

        #page-info {
            font-size: 1.3rem;
            color: var(--text-main);
            min-width: 60px;
            text-align: center;
            opacity: 0.6;
            font-family: 'Gamja Flower', cursive;
        }

        .add-page-trigger {
            font-size: 1.3rem;
            color: var(--deep-pink);
            cursor: pointer;
            opacity: 0.7;
            transition: opacity 0.2s;
            font-family: 'Gamja Flower', cursive;
        }

        .add-page-trigger:hover {
            opacity: 1;
        }

        .fab {
            position: fixed;
            bottom: 3rem;
            right: calc(50% - 460px); 
            width: 55px;
            height: 55px;
            background-color: var(--deep-pink);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 8px 20px rgba(255, 143, 163, 0.3);
            cursor: pointer;
            z-index: 100;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .fab:hover {
            transform: scale(1.1) rotate(90deg);
        }

        @media (max-width: 1000px) {
            .fab { right: 2rem; bottom: 2rem; }
            .notebook { padding-bottom: 140px; }
        }

        .delete-btn {
            position: absolute;
            top: 0.75rem;
            right: 0.75rem;
            font-size: 0.8rem;
            color: #f0f0f0;
            cursor: pointer;
        }

        .book-tab:hover .delete-btn {
            color: #ccc;
        }
    </style>
</head>
<body>

    <div class="notebook">
        <header>
            <h1 id="header-title">2026 사각사각-1</h1>
        </header>

        <main>
            <div id="book-list">
                <!-- 기록 카드들 -->
            </div>
        </main>

        <div class="pagination-container">
            <div id="new-page-hint" class="add-page-trigger" onclick="createNewEmptyPage()">
                + 새 페이지 펼치기
            </div>
            
            <div class="pagination-wrapper">
                <div id="prev-btn" class="page-btn" onclick="changePage(-1)">
                    <i class="fas fa-arrow-left"></i> 이전
                </div>
                <div id="page-info">01 / 01</div>
                <div id="next-btn" class="page-btn" onclick="changePage(1)">
                    다음 <i class="fas fa-arrow-right"></i>
                </div>
            </div>
        </div>
    </div>

    <div class="fab" onclick="addNewBookTab()">
        <i class="fas fa-plus text-xl"></i>
    </div>

    <script>
        let allBooks = []; 
        let currentPage = 1;
        let totalCreatedPages = 1; 

        function addNewBookTab() {
            const id = Date.now();
            const newBook = {
                id: id,
                cover: '',
                rating: 0,
                review: '',
                date: new Date().toLocaleDateString('ko-KR'),
                page: currentPage 
            };
            
            const insertIndex = allBooks.findIndex(b => b.page === currentPage);
            if (insertIndex === -1) {
                allBooks.push(newBook);
            } else {
                allBooks.splice(insertIndex, 0, newBook);
            }
            
            renderBooks();

            setTimeout(() => {
                const textarea = document.querySelector(`[data-id="${id}"] .review-input`);
                if(textarea) textarea.focus();
            }, 100);
        }

        function createNewEmptyPage() {
            totalCreatedPages++;
            currentPage = totalCreatedPages;
            renderBooks();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function changePage(direction) {
            const newPage = currentPage + direction;
            if (newPage >= 1 && newPage <= totalCreatedPages) {
                currentPage = newPage;
                renderBooks();
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }
        }

        function setRating(id, rating) {
            const book = allBooks.find(b => b.id === id);
            if (book) {
                book.rating = rating;
                renderBooks();
            }
        }

        function updateReview(id, text) {
            const book = allBooks.find(b => b.id === id);
            if (book) book.review = text;
        }

        function handleImageUpload(id, event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const book = allBooks.find(b => b.id === id);
                    if (book) {
                        book.cover = e.target.result;
                        renderBooks();
                    }
                };
                reader.readAsDataURL(file);
            }
        }

        function deleteTab(id) {
            if(!confirm('기록을 삭제할까요?')) return;
            allBooks = allBooks.filter(b => b.id !== id);
            renderBooks();
        }

        function renderBooks() {
            const list = document.getElementById('book-list');
            const pageInfo = document.getElementById('page-info');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const newPageHint = document.getElementById('new-page-hint');
            const headerTitle = document.getElementById('header-title');
            
            headerTitle.innerText = `2026 사각사각-${currentPage}`;

            const currentPageBooks = allBooks.filter(book => book.page === currentPage);

            prevBtn.className = `page-btn ${currentPage === 1 ? 'disabled' : ''}`;
            nextBtn.className = `page-btn ${currentPage === totalCreatedPages ? 'disabled' : ''}`;
            
            newPageHint.style.display = (currentPage === totalCreatedPages) ? 'block' : 'none';

            const curP = String(currentPage).padStart(2, '0');
            const maxP = String(totalCreatedPages).padStart(2, '0');
            pageInfo.innerText = `${curP} / ${maxP}`;

            if (currentPageBooks.length === 0) {
                list.innerHTML = `
                    <div class="flex flex-col items-center justify-center py-40 opacity-10">
                        <i class="fas fa-feather-alt text-4xl mb-6"></i>
                        <p class="text-sm tracking-[0.5em] uppercase gamja-flower" style="font-size:1.8rem">첫 문장을 기다려요</p>
                    </div>
                `;
                return;
            }

            list.innerHTML = currentPageBooks.map(book => `
                <div class="book-tab" data-id="${book.id}">
                    <div class="delete-btn" onclick="deleteTab(${book.id})"><i class="fas fa-times"></i></div>
                    
                    <div class="cover-slot" onclick="document.getElementById('file-${book.id}').click()">
                        ${book.cover ? 
                            `<img src="${book.cover}" alt="Book Cover">` : 
                            `<i class="fas fa-plus text-gray-100 text-xl"></i>`
                        }
                        <input type="file" id="file-${book.id}" class="hidden" accept="image/*" onchange="handleImageUpload(${book.id}, event)">
                    </div>

                    <div class="flex-grow">
                        <div class="flex justify-between items-center">
                            <div class="star-rating">
                                ${[1, 2, 3, 4, 5].map(num => `
                                    <i class="fas fa-star ${num <= book.rating ? 'active' : ''}" 
                                       onclick="setRating(${book.id}, ${num})"></i>
                                `).join('')}
                            </div>
                            <span class="text-[12px] text-gray-300 gamja-flower tracking-widest" style="font-size:1rem">${book.date}</span>
                        </div>
                        
                        <textarea 
                            class="review-input" 
                            rows="2" 
                            placeholder="사각사각, 기록을 남겨보세요..."
                            onblur="updateReview(${book.id}, this.value)"
                        >${book.review}</textarea>
                    </div>
                </div>
            `).join('');
        }

        renderBooks();
    </script>
</body>
</html>
