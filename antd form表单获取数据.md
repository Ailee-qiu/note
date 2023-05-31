### antd form表单获取数据

````tsx
//使用antd提供的钩子
const [form] = Form.useForm()
    const queryProject = (value: any)=>{
        const param = form.getFieldsValue()
        console.log(value)
        console.log(param)
    }
<Form form={form} onFinish={queryProject}>
    <Form.Item name={"project"}>
        <Input
            placeholder={'项目名'}
            type='text'
            /> 
     </Form.Item>
     <Form.Item name={"list"}>
         <Select 
                options={options}
                />
      </Form.Item>
      <Form.Item>
          <Button
              //button在里面的时候，htmlType得注明，点击按钮时才能触发form表单的onFinish
              type='primary' 
              htmlType='submit' 
              style={{margin:40}}>
                    查询
          </Button>
       </Form.Item>
</Form>
````

````tsx
//使用useRef

````



