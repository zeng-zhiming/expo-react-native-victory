# expo-react-native-victory

本库是在victory-native库上进行扩展，为 expo 和 react native 项目提供Victory用于实现图标功能，增加了对react-native-web的支持。

## 安装
`$ npx expo install expo-react-native-victory`
或

`$ yarn add expo-react-native-victory`

或

`$ npm install expo-react-native-victory --save`

## Usage
```javascript

import React, {Component} from 'react';
import {
    SafeAreaView,
    StyleSheet,
    Text,
    View,
    TouchableOpacity,
    Platform,
} from 'react-native';

import { VictoryPie } from 'expo-react-native-victory';

class TestPage extends Component {

    // 构造函数
    constructor(props, context) {
        super(props, context);

        this.state = {};
    }

    onPress = () => {
        alert('点击按钮');
    };

    render() {

        const dataList = [
            { x: '1', y: 1, label: "Title1" },
            { x: '2', y: 1, label: "Title2" },
            { x: '3', y: 1, label: "Title3" },
            { x: '4', y: 1, label: "Title4" },
            { x: '5', y: 1, label: "Title5" },
            { x: '6', y: 1, label: "Title6" },
            { x: '7', y: 1, label: "Title7" },
            { x: '8', y: 1, label: "Title8" },
            { x: '9', y: 1, label: "Title9" },
            { x: '10', y: 1, label: "Title10" },
        ];

        return (
            <SafeAreaView style={{flex: 1}}>
                <View style={styles.container}>
                    <TouchableOpacity onPress={this.onPress}>
                        <Text>点击按钮</Text>
                    </TouchableOpacity>
                    <View>
                        <VictoryPie
                            data={dataList}
                            labelRadius={({ innerRadius }) => 90 }
                            style={{ labels: { fill: "white", fontSize: 12} }}
                            innerRadius={80}
                            padAngle={({ datum }) => 1}
                            labelPlacement={() => "parallel"}
                            events={[
                                {
                                    // childName: 'all',	// 可选all或者子组件的name
                                    target: 'data',     // 可选parent、data、labels
                                    eventHandlers: Platform.OS ==="web" ? { // 适配Web点击事件
                                        onClick: (event,conf, index) => {	// 支持的事件都行，注意移动端和web端的不同就行

                                            console.log('onPress===>',dataList[index]);

                                            return [
                                                {
                                                    target: "data",
                                                    mutation: ({ style }) => {
                                                        return style.fill === "#c43a31" ? null : { style: { fill: "#c43a31" } };
                                                    }
                                                }, {
                                                    target: "labels",
                                                    mutation: ({ text }) => {
                                                        return text === "clicked" ? null : { text: "clicked" };
                                                    }
                                                }
                                            ];
                                        },
                                    } : {
                                        onPress: (event,conf, index) => {	// 支持的事件都行，注意移动端和web端的不同就行

                                            console.log('onPress===>',dataList[index]);

                                            return [
                                                {
                                                    target: "data",
                                                    mutation: ({ style }) => {
                                                        return style.fill === "#c43a31" ? null : { style: { fill: "#c43a31" } };
                                                    }
                                                }, {
                                                    target: "labels",
                                                    mutation: ({ text }) => {
                                                        return text === "clicked" ? null : { text: "clicked" };
                                                    }
                                                }
                                            ];
                                        },
                                    },
                                },
                            ]}
                        />
                    </View>
                </View>
            </SafeAreaView>
        );
    }
}


const styles = StyleSheet.create({
    container: {
        height: '100%',
        justifyContent: 'center',
        alignItems: 'center',
    },
});


export default TestPage;
```
