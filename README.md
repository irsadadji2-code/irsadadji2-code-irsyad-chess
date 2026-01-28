                        const moves = getLegalMoves(r, c);
                        if (moves.length > 0) return false;
                    }
                }
            }
            return true;
        }

        function isDraw() {
            // Simplified draw: no legal moves and not in check
            for (let r = 0; r < 8; r++) {
                for (let c = 0; c < 8; c++) {
                    if (board[r][c] && isPieceColor(board[r][c], currentTurn)) {
                        if (getLegalMoves(r, c).length > 0) return false;
                    }
                }
            }
            return !isInCheck(currentTurn);
        }

        // =============================
        // COMPUTER AI (Simple but real)
        // =============================
        function computerMove() {
            let allMoves = [];
            for (let r = 0; r < 8; r++) {
                for (let c = 0; c < 8; c++) {
                    if (board[r][c] && isPieceColor(board[r][c], 'black')) {
                        const moves = getLegalMoves(r, c);
                        moves.forEach(m => {
                            allMoves.push({ from: [r, c], to: m });
                        });
                    }
                }
            }

            if (allMoves.length === 0) return;

            // Random legal move (aman, tidak error)
            const move = allMoves[Math.floor(Math.random() * allMoves.length)];
            movePiece(move.from[0], move.from[1], move.to[0], move.to[1]);
        }

        // =============================
        // NOTIFICATION
        // =============================
        function showNotification(text) {
            notificationElement.textContent = text;
            notificationElement.style.display = 'block';
            setTimeout(() => {
                notificationElement.style.display = 'none';
            }, 2000);
        }

        // =============================
        // TIMER
        // =============================
        function startTimer() {
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                timers[currentTurn]++;
                updateTimer();
            }, 1000);
        }

        function updateTimer() {
            const w = formatTime(timers.white);
            const b = formatTime(timers.black);
            timerElement.textContent = `White: ${w} | Black: ${b}`;
        }

        function formatTime(sec) {
            const m = String(Math.floor(sec / 60)).padStart(2, '0');
            const s = String(sec % 60).padStart(2, '0');
            return `${m}:${s}`;
        }

        // =============================
        // RESET & NEW GAME
        // =============================
        function resetGame() {
            board = [
                ['r','n','b','q','k','b','n','r'],
                ['p','p','p','p','p','p','p','p'],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['P','P','P','P','P','P','P','P'],
                ['R','N','B','Q','K','B','N','R']
            ];
            currentTurn = 'white';
            gameOver = false;
            timers = { white: 0, black: 0 };
            clearSelection();
            initBoard();
            startTimer();
            statusElement.textContent = "White's turn";
        }

        newGameBtn.onclick = resetGame;
        resetBtn.onclick = resetGame;

        // =============================
        // MODE SWITCH
        // =============================
        document.querySelectorAll('input[name="mode"]').forEach(radio => {
            radio.addEventListener('change', e => {
                mode = e.target.value;
                roomInput.style.display = mode === 'multi' ? 'block' : 'none';
            });
        });

        // =============================
        // INIT GAME
        // =============================
        initBoard();
        startTimer();
    </script>
</body>
</html>
