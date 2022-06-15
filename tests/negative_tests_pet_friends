from api import PetFriends
from settings import valid_email, valid_password, invalid_email, invalid_password, invalid_auth_key
import os

pf = PetFriends()


#1 Негативный тест на запрос ключа с невалидной почтой
def test_get_api_key_with_invalid_email(email=invalid_email, password=valid_password):

    status, result = pf.get_api_key(email, password)

    assert status == 403


#2 Негативный тест на запрос ключа с невалидным паролем
def test_get_api_key_with_invalid_pass(email=valid_email, password=invalid_password):

    status, result = pf.get_api_key(email, password)

    assert status == 403


#3 Негавтиный тест на получение списка питомцев с невалидным фильтром
def test_get_all_pets_with_invalid_filter(filter='dogs'):

    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.get_list_of_pets(auth_key, filter)

    assert status == 500


#4 Негативный тест на получение списка питомцев с невалидным ключом
def test_get_all_pets_with_invalid_key(filter=''):

    status, result = pf.get_list_of_pets(invalid_auth_key, filter)

    assert status == 403


#5 Негативный тест на добавление питомца с фото неверного формата (здесь баг, т.к. с файлом .xls получаем код 200)
def test_add_new_pet_with_invalid_photo(name='Оби', animal_type='шимпанзе',
                                     age='3', pet_photo='images/dz.xls'):

    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.add_new_pet(auth_key, name, animal_type, age, pet_photo)

    assert status == 400


#6 Негативный тест на добавление питомца с пустым имене(присутствует баг, т.к. с пустым именем происходит добавление питомца)
def test_add_new_pet_with_empty_name(name='', animal_type='шимпанзе',
                                     age='3', pet_photo='images/15.jpg'):

    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.add_new_pet(auth_key, name, animal_type, age, pet_photo)

    assert status == 400


#7 Негативный тест на добавление питомца с пустым полем породы(баг, аналогичный в тесте №6)
def test_add_new_pet_with_empty_animal_type(name='Оби', animal_type='',
                                     age='3', pet_photo='images/15.jpg'):

    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.add_new_pet(auth_key, name, animal_type, age, pet_photo)

    assert status == 400


#8 Негативный тест на добавление питомца с невалидным значением возраста(баг,т.к. добавляет питомца с текстовым значением возраста)
def test_add_new_pet_with_invalid_type_of_age(name='Оби', animal_type='шимпанзе',
                                     age='пятнадцать', pet_photo='images/15.jpg'):

    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.add_new_pet(auth_key, name, animal_type, age, pet_photo)

    assert status == 400


#9 Негативный тест на добавление фото питомцу с невалидным id из моего списка
def test_add_photo_of_pet_with_invalid_id(pet_photo='images/6f.jpeg'):

    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)

    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")

    if len(my_pets['pets']) > 0:
        pet_id = my_pets['pets'][-999]['id'] #невалидный id
        status, result = pf.add_photo_of_pet(auth_key, pet_id, pet_photo)

        assert status == 500
    else:
        # если спиок питомцев пустой, то выкидываем исключение с текстом об отсутствии своих питомцев
        raise Exception("There is no my pets")


#10 Негативный тест на обновление информации с невалидным значением возраста(баг,т.к. запрос меняет возраст питомца на текстовое значение)
def test_successful_update_self_pet_info_with_invalid_type_of_age(name='Мурзик', animal_type='Котэ', age='cемь'):

    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")

    if len(my_pets['pets']) > 0:
        status, result = pf.update_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)

        assert status == 400
    else:
        # если спиок питомцев пустой, то выкидываем исключение с текстом об отсутствии своих питомцев
        raise Exception("There is no my pets")
