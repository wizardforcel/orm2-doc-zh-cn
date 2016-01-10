# hasMany（多对多关系）

> 译者：[飞龙](https://github.com/wizardforcel)

> 来源：[hasMany](https://github.com/dresende/node-orm2/wiki/hasMany)

## hasMany

是多对多的关系（包括连接表）。

例如：`Patient.hasMany('doctors', Doctor, { why: String }, { reverse: 'patients', key: true })`。

病人可以拥有许多不同的医生。每个医生可以拥有许多不同的病人。

当你调用`Patient.sync()`时，会创建一个连接表`patient_doctors`。

| 列名称 | 类型 |
| --- | --- |
| patient_id | Integer |
| doctor_id | Integer |
| why | varchar(255) |

下列函数是可用的：

```
// 获取所有关联医生的列表
patient.getDoctors(function(err, doctors) {
  // ...
});

// 向连接表中增加记录
patient.addDoctors([phil, bob], function(err) {
  // ...
});

// 移除连接表中的现有记录，并增加新的
patient.setDoctors([phil, nephewOfBob], function(err) {
  // ...
});

// 检查是否某个病人关联了指定的医生
patient.hasDoctors([bob], function(err, patientHasBobAsADoctor) {
  // because that is a totally legit and descriptive variable name
  if (patientHasBobAsADoctor) {
    // ...
  } else {
    // ...
  }
});

// 从连接表中移除指定记录
patient.removeDoctors([bob], function(err) {
  // ...
});

// 并且所有医生都有自己的方法来获取病人
bob.getPatients(function(err, patients) {
  if (patients.indexOf(you) !== -1) {
    // woot!
  } else {
    // ...
  }
});

// 以及其他
```

要把医生关联到病人：

```
patient.addDoctor(surgeon, {why: 'remove appendix'}, function(err) {
  // ...
});

// or...
surgeon.addPatient(patient, {why: 'remove appendix'}, function(err) {
  // ...
});
```

这样会添加`{patient_id: 4, doctor_id: 6, why: "remove appendix"}`到连接表中。

## API

```
Model.hasMany(
  name,       // String. 关联名称
  otherModel, // Model. 要关联的模型
  extraProps, // Object. 在连接表上出现的额外属性
  opts        // Object. 关联的选项
);
```

## 选项

| 选项名称 | 类型 | 描述 |
| --- | --- | --- |
| autoFetch | Boolean | 默认为`false`。如果为`true`，关联将会自动被获取。 |
| autoFetchLimit | Number | 默认为`1`。自动获取的深度。 |
| key | Boolean | 默认为`false`（由于历史原因）。如果为`true`，表中外键的列会形成一个组合键。 |
| mergeTable | String | 连接表的自定义名称 |
| mergeId | String | 代表当前模型那一列的自定义名称 |
| mergeAssocId | String | 代表另一个模型那一列的自定义名称 |
| reverse | String | 默认为`false`。如果为`true`，关联可以通过另一个模型使用指定方法获取到。 |
| getAccessor | String | 默认为`'get' + Name`。允许重命名关联访问器。 |
| setAccessor | String | 默认为`'set' + Name`。允许重命名关联访问器。 |
| hasAccessor | String | 默认为`'has' + Name`。允许重命名关联访问器。 |
| delAccessor | String | 默认为`'del' + Name`。允许重命名关联访问器。 |
| addAccessor | String | 默认为`'add' + Name`。允许重命名关联访问器。 |
