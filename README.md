
def print_rules():
    # выводим на консоль правила игры
    print('Правила игры\n'
          'Игроки по очереди ставят знаки на свободные клетки поля. Первый, выстроивший\n'
          'в ряд 3 своих фигуры по вертикали, горизонтали или диагонали, выигрывает.\n'
          'Первый ход делает игрок, ставящий крестики.')


def game_board():
    # выводим на консоль игровое поле
    print()
    print('%+2s |%+2s |%+2s' % (list_game_board[1], list_game_board[2], list_game_board[3]))
    print('---+---+---')
    print('%+2s |%+2s |%+2s' % (list_game_board[4], list_game_board[5], list_game_board[6]))
    print('---+---+---')
    print('%+2s |%+2s |%+2s' % (list_game_board[7], list_game_board[8], list_game_board[9]))
    print()


def choice_sign():
    # игрок выбирает Х или 0
    while True:
        sign = input('Добро пожаловать в игру!' 
                     'Выберите "x" или "o": ').lower()
        if sign in 'xXoO':
            break
    return ('x', 'o') if sign in 'xX' else ('o', 'x')


def player_move(cell):
    # функция позволяет сделать ход игроку
    while cell not in [str(i) for i in range(1, 10)]:
        cell = input('Такой клетки нет! Выберите другую клетку. ' + '\n' + 'Введите цифру (1-9): ')
    while list_game_board[int(cell)] in (sign_player1 + sign_player2):
        cell = input('Клетка заполнена! Выберите другую клетку. ' + '\n' + 'Введите цифру (1-9): ')
    if turn == 1:
        list_game_board[int(cell)] = sign_player1
    else:
        list_game_board[int(cell)] = sign_player2


def bot_move():
    pass


def victory():
    # функция с условиями победы
    comb_sings = sign_player1 * 3 if turn == 1 else sign_player2 * 3

    if comb_sings in ((list_game_board[1] + list_game_board[2] + list_game_board[3]),
                      (list_game_board[4] + list_game_board[5] + list_game_board[6]),
                      (list_game_board[7] + list_game_board[8] + list_game_board[9]),
                      (list_game_board[1] + list_game_board[4] + list_game_board[7]),
                      (list_game_board[2] + list_game_board[5] + list_game_board[8]),
                      (list_game_board[3] + list_game_board[6] + list_game_board[9]),
                      (list_game_board[1] + list_game_board[5] + list_game_board[9]),
                      (list_game_board[3] + list_game_board[5] + list_game_board[7])):
        print('Выиграл игрок №' + str(turn) + '!')
        return True
    elif move_count == 8:
        print('Ничья!')
        return True


new_game = 1
game = 1

while True:
    if new_game:
        print_rules()
    print('\nПартия:', game)

    list_game_board = [str(i) for i in range(10)]
    game_board()
    sign_player1, sign_player2 = choice_sign()
    turn = 1 if sign_player1 == 'x' else 2
    move_count = 0

    while True:
        move_count += 1
        player_move(input('Ходит игрок №' + str(turn) + '\n' + 'Введите цифру (1-9): '))
        game_board()
        if victory():
            break
        turn = 1 if turn == 2 else 2

    if input('Сыграем снова? Нажмите "Y" если ДА или "N" если НЕТ)').lower() not in 'Yyesla':
        print('Игра завершена')
        break
    new_game = 0
    game += 1
