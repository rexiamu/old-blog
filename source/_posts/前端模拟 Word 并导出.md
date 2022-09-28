---
title: 前端模拟 Word 并导出
date: 2022-09-28 14:59:55
tags:
  - Word
categories:
  - 前端
---

![Word](https://s2.loli.net/2022/09/28/prNqX3Zz4WxMn5J.webp)

<!-- more -->

## 全部代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
  <title>摸鱼的日常</title>
  <style>
    html {
      background-color: #666;
    }

    #page {
      width: 550px;
      margin: 50px auto;
      padding: 40px 120px;
      background-color: #fff;
      box-shadow: 0 0 50px #333;
    }

    p {
      text-indent: 2em;
      line-height: 2em;
    }

    table {
      width: 100%;
      text-align: center;
      border-width: 1px;
      border-color: #000;
      border-collapse: collapse;
    }

    .btn-export {
      position: fixed;
      top: 20pt;
      right: 20pt;
    }

    .btn-edit {
      position: fixed;
      top: 50pt;
      right: 20pt;
    }

    .img {
      display: none;
    }
  </style>
</head>

<body>
  <div id="app">
    <div id="page">
      <h1 style="font-size: 20pt;margin-bottom: 20px;">
        <center><b>摸鱼的日常</b></center>
      </h1>

      <p>
        <center>2022年9月20日</center>
      </p>

      <h2>一、标题</h2>

      <h3>（一）段落加表格</h3>

      <p>
        南门外那些水坑，哪个坑里有嘛鱼，哪个坑里的鱼大小，哪个坑的鱼有多少条，他心里全一清二楚。
        他能把坑里的鱼全钓绝了，但他也决不把任何一个坑里的鱼钓绝了。钓绝了，他玩嘛？
      </p>

      <table border="1" align="center">
        <thead>
          <tr>
            <th>序号</th>
            <th>喝水</th>
            <th>吃点心</th>
            <th>打盹儿</th>
            <th>发呆</th>
          </tr>
        </thead>

        <tbody>
          <tr>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
          </tr>
        </tbody>

        <tfoot>
          <tr>
            <td>合计</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
          </tr>
        </tfoot>
      </table>

      <h3>（二）段落加图</h3>

      <p>
        像是掌心里的一条鱼。攥紧时它会拼命挣扎，把它放回河流中，它又不知游去了什么方向。
        这样拼命克制。又无法不放任它随它而去。
      </p>

      <center>
        <img class="img" width="550">
      </center>

      <div id="chart" class="chart" style="width: 550px;height:300px;"></div>

    </div>

    <el-button class="btn-export" type="success" size="small" icon="el-icon-download" @click="html2word">导出
    </el-button>

    <el-button class="btn-edit" type="primary" size="small" icon="el-icon-edit" @click="showEditPanel = true">编辑
    </el-button>

    <el-drawer :visible.sync="showEditPanel">
      <el-form ref="timeConsumptionForm" :model="timeConsumptionForm" label-width="80px" size="small">
        <el-form-item label="喝水">
          <el-input type="number" v-model="timeConsumptionForm.drink"></el-input>
        </el-form-item>
        <el-form-item label="吃点心">
          <el-input type="number" v-model="timeConsumptionForm.eat"></el-input>
        </el-form-item>
        <el-form-item label="打盹儿">
          <el-input type="number" v-model="timeConsumptionForm.sleep"></el-input>
        </el-form-item>
        <el-form-item label="发呆">
          <el-input type="number" v-model="timeConsumptionForm.daze"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="updateTimeConsumptionForm">确定</el-button>
        </el-form-item>
      </el-form>
    </el-drawer>

  </div>

</body>
<script src="https://cdn.jsdelivr.net/npm/echarts@5.3.3/dist/echarts.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
<script>
  const timeConsumptionMap = {
    drink: '喝水',
    eat: '吃点心',
    sleep: '打盹儿',
    daze: '发呆'
  };

  const app = new Vue({
    el: '#app',
    data: {
      chart: null,
      timeConsumption: [
        { value: 1, name: '喝水' },
        { value: 1.5, name: '吃点心' },
        { value: 2, name: '打盹儿' },
        { value: 3.5, name: '发呆' }
      ],
      timeConsumptionForm: {
        drink: 1,
        eat: 1.5,
        sleep: 2,
        daze: 3.5
      },
      showEditPanel: false
    },
    mounted() {
      // 初始化图
      this.chart = echarts.init(document.getElementById('chart'));

      // 渲染数据
      this.updateChart();

      // 渲染完成，转为图片
      this.chart.on('finished', () => {
        let img = document.querySelector('.img');
        img.src = this.chart.getConnectedDataURL({
          pixelRatio: 2
        });
      });
    },
    methods: {
      updateTimeConsumptionForm() {
        this.updateChart();
      },
      html2word() {
        let header = `<html xmlns:o='urn:schemas-microsoft-com:office:office'
        xmlns:w='urn:schemas-microsoft-com:office:word'
        xmlns='http://www.w3.org/TR/REC-html40'>
        <head>
          <!--[if gte mso 9]><xml><w:WordDocument><w:View>Print</w:View><w:TrackMoves>false</w:TrackMoves><w:TrackFormatting/><w:ValidateAgainstSchemas/><w:SaveIfXMLInvalid>false</w:SaveIfXMLInvalid><w:IgnoreMixedContent>false</w:IgnoreMixedContent><w:AlwaysShowPlaceholderText>false</w:AlwaysShowPlaceholderText><w:DoNotPromoteQF/><w:LidThemeOther>EN-US</w:LidThemeOther><w:LidThemeAsian>ZH-CN</w:LidThemeAsian><w:LidThemeComplexScript>X-NONE</w:LidThemeComplexScript><w:Compatibility><w:BreakWrappedTables/><w:SnapToGridInCell/><w:WrapTextWithPunct/><w:UseAsianBreakRules/><w:DontGrowAutofit/><w:SplitPgBreakAndParaMark/><w:DontVertAlignCellWithSp/><w:DontBreakConstrainedForcedTables/><w:DontVertAlignInTxbx/><w:Word11KerningPairs/><w:CachedColBalance/><w:UseFELayout/></w:Compatibility><w:BrowserLevel>MicrosoftInternetExplorer4</w:BrowserLevel><m:mathPr><m:mathFont m:val="Cambria Math"/><m:brkBin m:val="before"/><m:brkBinSub m:val="--"/><m:smallFrac m:val="off"/><m:dispDef/><m:lMargin m:val="0"/> <m:rMargin m:val="0"/><m:defJc m:val="centerGroup"/><m:wrapIndent m:val="1440"/><m:intLim m:val="subSup"/><m:naryLim m:val="undOvr"/></m:mathPr></w:WordDocument></xml><![endif]-->
          <meta charset='utf-8'>
          <style>
            p {
              text-indent: 2em;
              line-height: 2em;
            }
            table {
              width: 100%;
              text-align: center;
              border-width: 1px;
              border-color: #000;
              border-collapse: collapse;
            }
            .chart {
              display: none;
            }
          </style>
        </head><body>`;
        let footer = "</body></html>";
        let sourceHTML = header + document.getElementById("page").innerHTML + footer;

        let source = 'data:application/vnd.ms-word;charset=utf-8,' + encodeURIComponent(sourceHTML);
        let fileDownload = document.createElement("a");
        document.body.appendChild(fileDownload);
        fileDownload.href = source;
        fileDownload.download = '摸鱼的日常.doc';
        fileDownload.click();
        document.body.removeChild(fileDownload);
      },
      updateChart() {
        // 指定图表的配置项和数据
        let option = {
          label: {
            formatter: '{b}: {c}'
          },
          series: [
            {
              name: '统计',
              type: 'pie',
              radius: '50%',
              data: Object.keys(this.timeConsumptionForm).map(key => {
                return {
                  value: this.timeConsumptionForm[key],
                  name: timeConsumptionMap[key]
                }
              })
            }
          ]
        };

        // 渲染数据
        this.chart.setOption(option);
      }
    }
  })
</script>
</html>
```
