<template>
    <header class="header">
        <div class="header-title">Документы</div>
        <div class="header-action">
            <button class="action-btn action-bookmark"></button>
            <button class="action-btn action-type action-btn-plus">Новый тип</button>
            <button class="action-btn action-doc action-btn-plus">Новый документ</button>
        </div>
    </header>
</template>
<style scoped>
.header {
    padding: 33px 30px 24px;
    color: #000;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header-title {
    font-weight: 500;
    font-size: 22px;
    line-height: 108%;
}

.header-action {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 10px;
}

.action-btn {
    padding: 8px 10px 9px;
    height: 30px;
    border: 1px solid #d3d8df;
    border-radius: 50px;
    font-weight: 500;
    font-size: 12px;
    line-height: 1;
    cursor: pointer;
    outline: none;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #fff;
}

.action-bookmark {
    width: 30px;
}

.action-bookmark::before {
    content: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTIiIGhlaWdodD0iMTUiIHZpZXdCb3g9IjAgMCAxMiAxNSIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTExIDEzLjg1NzFMNiAxMC4yODU3TDEgMTMuODU3MVYyLjQyODU3QzEgMi4wNDk2OSAxLjE1MDUxIDEuNjg2MzMgMS40MTg0MiAxLjQxODQyQzEuNjg2MzMgMS4xNTA1MSAyLjA0OTY5IDEgMi40Mjg1NyAxSDkuNTcxNDNDOS45NTAzMSAxIDEwLjMxMzcgMS4xNTA1MSAxMC41ODE2IDEuNDE4NDJDMTAuODQ5NSAxLjY4NjMzIDExIDIuMDQ5NjkgMTEgMi40Mjg1N1YxMy44NTcxWiIgc3Ryb2tlPSIjOEU5Q0JCIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIvPgo8L3N2Zz4K);
    margin-top: 4px;
}

.action-btn-plus {
    padding: 8px 20px 9px 34px;
}

.action-btn-plus::before {
    content: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTQiIGhlaWdodD0iMTUiIHZpZXdCb3g9IjAgMCAxNCAxNSIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTYuOTk2OTcgMTQuMDA4M0M2LjQ0NjgxIDE0LjAwODMgNi4wMDA4MiAxMy41NTk1IDYuMDAwODIgMTMuMDA1OUw1Ljk5OTgzIDEuMDEwNzFDNS45OTk4MyAwLjQ1NzA5NiA2LjQ0NTgyIDAuMDA4MzAwNzggNi45OTU5OSAwLjAwODMwMDc4QzcuNTQ2MTUgMC4wMDgzMDA3OCA3Ljk5MjE1IDAuNDU3MDk3IDcuOTkyMTUgMS4wMTA3MUw3Ljk5MzEzIDEzLjAwNTlDNy45OTMxMyAxMy41NTk1IDcuNTQ3MTQgMTQuMDA4MyA2Ljk5Njk3IDE0LjAwODNaIiBmaWxsPSIjMDA2NkZGIi8+CjxwYXRoIGQ9Ik0xNCA3LjAxNjg4QzE0IDYuNDYzMjYgMTMuNTU0IDYuMDE0NDcgMTMuMDAzOCA2LjAxNDQ3TDAuOTk2MTU4IDYuMDE0NDhDMC40NDU5OTUgNi4wMTQ0OCAwIDYuNDYzMjcgMCA3LjAxNjg5QzAgNy41NzA1MSAwLjQ0NTk5NSA4LjAxOTMgMC45OTYxNTkgOC4wMTkzTDEzLjAwMzggOC4wMTkzQzEzLjU1NCA4LjAxOTMgMTQgNy41NzA1IDE0IDcuMDE2ODhaIiBmaWxsPSIjMDA2NkZGIi8+Cjwvc3ZnPgo=);
    position: absolute;
    top: 50%;
    left: 10px;
    transform: translateY(-50%);
    margin-top: 2px;
}
</style>