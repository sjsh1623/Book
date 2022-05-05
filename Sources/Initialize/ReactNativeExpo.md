# React Native initialization with Expo

## First what is expo
> Expo is a framework and a platform for universal React applications. It is a set of tools and services built around React Native and native platforms that help you develop, build, deploy, and quickly iterate on iOS, Android, and web apps from the same JavaScript/TypeScript codebase.
> (https://docs.expo.dev/)

> Expo는 React application을 위한 프레임워크이자 플랫폼이다. Expo는 React 또는 React Native 풀랫폼을 iOS, Android , Web app으로  Javascript / Typescript 기반으로 개발, 빌드, 배포를 빠르고 쉽게 설정할 수 있도록 도와준다.

## 상황
> React Native 프로젝트를 생성하였지만 폴더가 이미 존재한다는 오류가 났다. (IntelliJ)

## 해결
```
# expo-cli 설치
npm install --global expo-cli

# 새로운 프로젝트를 생성
expo init my-project
```