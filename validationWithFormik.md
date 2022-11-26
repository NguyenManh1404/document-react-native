# Validation with formik
1. **yarn add formik** : install library
2. **yarn add yup** : install library
3. **Example use:**
```js
import { View, TextInput, StyleSheet } from 'react-native';
import { useFormik } from 'formik';
import * as Yup from 'yup';

const SignUpScreen = () => {

const formik = useFormik({
    initialValues: {
      firstName: '',
      lastName: '',
      email: '',
      password: '',
      confirmPassword: '',
      avatar: null,
    },
    validationSchema: Yup.object({
      firstName: Yup.string()
        .trim(translate('firstNameWhileSpace'))
        .strict(true)
        .matches(/^[a-zA-Z]+(([',. -’][a-zA-Z ])?[a-zA-Z]*)*$/g, translate('firstNameInvalid'))
        .min(2, translate('nameMin'))
        .max(70, translate('nameMax'))
        .required(translate('firstNameRequired')),

      lastName: Yup.string()
        .trim(translate('lastNameWhileSpace'))
        .strict(true)
        .matches(/^[a-zA-Z]+(([',. -’][a-zA-Z ])?[a-zA-Z]*)*$/g, translate('lastNameInvalid'))
        .min(2, translate('lastNameMin'))
        .max(70, translate('lastNameMax'))
        .required(translate('lastNameRequired')),
      email: Yup.string().email(translate('emailInvalid')).required(translate('emailRequired')),
      password: Yup.string()
        .min(6, translate('passwordMinMax'))
        .max(20, translate('passwordMinMax'))
        .required(translate('passwordRequired')),
      confirmPassword: Yup.string()
        .min(6, translate('confirmPasswordMinMax'))
        .max(20, translate('confirmPasswordMinMax'))
        .required(translate('confirmPasswordRequired'))
        .oneOf([Yup.ref('password'), null], translate('confirmPasswordNotMatch')),
    }),
    onSubmit: (values) => {
      onSignUp(values);
    },
  });

    return(
        <View>
            <TextInput
            />
        </View>
    )
}
export default LoginScreen;
const styles = StyleSheet.create({})
```
4. demo sample

```js
import {
  StyleSheet,
  Text,
  View,
  Image,
  TouchableOpacity,
  TextInput,
  Alert,
} from 'react-native';
import React from 'react';
import { Formik } from 'formik';
import * as Yup from 'yup';

const App = () => {
  const initUser = {
    email: '',
    password: '',
  };

    const validationSchema = Yup.object({
      email: Yup.string().email('Email không đúng định dạng').required('Yêu cầu nhập email'),
      password: Yup.string()
        .min(6, 'Mật khẩu quá ngắn')
        .max(20, 'Mật khẩu qúa dài')
        .required('Mật khẩu không được để trống'),
    });

    const login = (values) => {
      Alert.alert('Hello', values?.email +":)");
    }

  return (
    <View style={styles.container}>
      <Formik
        initialValues={initUser}
        validationSchema={validationSchema}
        onSubmit={values => {
          console.log("🚀 ~ file: App.js ~ line 55 ~ App ~ values", values)
          login(values);
        }}>
        {({
          values,
          errors,
          touched,
          handleChange,
          handleBlur,
          handleSubmit,
        }) => {
          const {email,password} = values;
          return (
            <>
              <View style={styles.form}>
                <View>
                  <Text style={styles.text1}>XIN MỜI ĐĂNG NHẬP</Text>
                </View>
                <Image
                  style={styles.tinyLogo}
                  source={{
                    uri: 'https://cdn-icons-png.flaticon.com/512/295/295128.png',
                  }}></Image>
                <View style={styles.formcontrol}>
                  <Text type="email" name="email" style={styles.label}>
                    Email
                  </Text>
                  <TextInput
                    value={email}
                    onChangeText={handleChange('email')}
                    onBlur={handleBlur('email')}
                    style={styles.input}
                  />
                  {errors.email && touched.email ? (
                    <Text style={{color: 'red'}}>{errors.email}</Text>
                  ) : null}
                </View>
                <View style={styles.formcontrol}>
                  <Text type="password" name="password" style={styles.label}>
                    Password
                  </Text>
                  <TextInput
                    value={password}
                    onChangeText={handleChange('password')}
                    onBlur={handleBlur('password')}
                    secureTextEntry={true}
                    style={styles.input}
                  />
                  {errors.password && touched.password ? (
                    <Text style={{color: 'red'}}>{errors.password}</Text>
                  ) : null}
                </View>
                <TouchableOpacity
                  type="submit"
                  style={styles.button}
                  onPress={handleSubmit}>
                  <Text style={styles.textbutton}>Đăng nhập</Text>
                </TouchableOpacity>
              </View>
            </>
          );
        }}
      </Formik>
    </View>
  );
};
export default App;

const styles = StyleSheet.create({
  container: {
    marginTop: 200,
    backgroundColor: 'white',
    justifyContent: 'center',
    borderRadius: 10,
    elevation: 10,
    width: '95%',
    marginLeft: 10,
    shadowRadius: 8,
    shadowOffset: {width: 0, height: 2},
    shadowOpacity: 0.26,
    shadowColor: 'black',
  },
  tinyLogo: {
    width: 50,
    height: 50,
    marginLeft: 150,
    opacity: 10,
  },
  text1: {
    justifyContent: 'center',
    color: 'red',
    marginLeft: 90,
    fontWeight: 'bold',
    fontSize: 20,
  },
  label: {color: 'black', fontSize: 15, marginLeft: 10},
  text: {color: 'black', fontSize: 20},
  textbutton: {color: 'white', fontSize: 20},
  input: {
    width: '90%',
    paddingHorizontal: 2,
    paddingVertical: 5,
    borderBottomColor: '#ccc',
    borderBottomWidth: 1,
    marginLeft: 10,
    marginRight: 10,
  },
  button: {
    borderRadius: 30,
    width: 200,
    height: 50,
    backgroundColor: '#fc0345',
    padding: 10,
    alignItems: 'center',
    marginLeft: 95,
    marginTop: 20,
    marginBottom: 30,
    paddingTop: 10,
  },
});

```
