Схема баз данных
![image](https://github.com/user-attachments/assets/b17f5910-b4f9-4262-9c8b-9f42fef18710)
ППримеры классов generics: from django.contrib.auth.models import User
from rest_framework import generics, permissions, views
from . import serializers from .models import Post, Comment, Category from .serializers import PostSerializer from .permissions import IsOwnerOrReadOnly
Класс для отображения списка всех пользователей
class UserList(generics.ListAPIView): queryset = User.objects.all() # Запрос для получения всех пользователей serializer_class = serializers.UserSerializer # Сериализатор для пользователей
Класс для отображения деталей конкретного пользователя
class UserDetail(generics.RetrieveAPIView): queryset = User.objects.all() # Запрос для получения всех пользователей serializer_class = serializers.UserSerializer # Сериализатор для пользователей
Класс для отображения списка всех постов и создания новых постов
class PostList(generics.ListCreateAPIView): queryset = Post.objects.all() # Запрос для получения всех постов serializer_class = PostSerializer # Сериализатор для постов permission_classes = [permissions.IsAuthenticatedOrReadOnly] # Разрешения: только аутентифицированные пользователи могут создавать посты
# Метод для создания нового поста, устанавливает владельца поста
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
Класс для отображения, обновления и удаления конкретного поста
class PostDetail(generics.RetrieveUpdateDestroyAPIView): queryset = Post.objects.all() # Запрос для получения всех постов serializer_class = PostSerializer # Сериализатор для постов permission_classes = [permissions.IsAuthenticatedOrReadOnly, # Разрешения: только аутентифицированные пользователи могут обновлять и удалять посты IsOwnerOrReadOnly] # Разрешения: только владелец может обновлять и удалять пост
Класс для отображения списка всех комментариев и создания новых комментариев
class CommentList(generics.ListCreateAPIView): queryset = Comment.objects.all() # Запрос для получения всех комментариев serializer_class = serializers.CommentSerializer # Сериализатор для комментариев permission_classes = [permissions.IsAuthenticatedOrReadOnly] # Разрешения: только аутентифицированные пользователи могут создавать комментарии
# Метод для создания нового комментария, устанавливает владельца комментария
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
Класс для отображения, обновления и удаления конкретного комментария
class CommentDetail(generics.RetrieveUpdateDestroyAPIView): queryset = Comment.objects.all() # Запрос для получения всех комментариев serializer_class = serializers.CommentSerializer # Сериализатор для комментариев permission_classes = [permissions.IsAuthenticatedOrReadOnly, # Разрешения: только аутентифицированные пользователи могут обновлять и удалять комментарии IsOwnerOrReadOnly] # Разрешения: только владелец может обновлять и удалять комментарий
Класс для отображения списка всех категорий и создания новых категорий
class CategoryList(generics.ListCreateAPIView): queryset = Category.objects.all() # Запрос для получения всех категорий serializer_class = serializers.CategorySerializer # Сериализатор для категорий permission_classes = [permissions.IsAuthenticatedOrReadOnly] # Разрешения: только аутентифицированные пользователи могут создавать категории
# Метод для создания новой категории, устанавливает владельца категории
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
Класс для отображения, обновления и удаления конкретной категории
class CategoryDetail(generics.RetrieveUpdateDestroyAPIView): queryset = Category.objects.all() # Запрос для получения всех категорий serializer_class = serializers.CategorySerializer # Сериализатор для категорий permission_classes = [permissions.IsAuthenticatedOrReadOnly, # Разрешения: только аутентифицированные пользователи могут обновлять и удалять категории IsOwnerOrReadOnly] # Разрешения: только владелец может обновлять и удалять категорию
Назначение прав доступа (permissions) Примеры разрешений: from rest_framework import permissions
Разрешение, которое позволяет только владельцу объекта его редактировать или удалять
class IsOwnerOrReadOnly(permissions.BasePermission): def has_object_permission(self, request, view, obj): if request.method in permissions.SAFE_METHODS: return True return obj.owner == request.user
Применение разрешений в представлениях
class PostDetail(generics.RetrieveUpdateDestroyAPIView): queryset = Post.objects.all() serializer_class = PostSerializer permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly]
Работа в Postman Get запрос:
![image](https://github.com/user-attachments/assets/9881249a-04fa-4e7c-beff-ea9df67861ca)
Аутентификация: 
![image](https://github.com/user-attachments/assets/9eaa1fdb-90b8-4e88-a97e-44a65522a922)
