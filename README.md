# Performance-Optimize
## 1,Use Lightweight Third Party Libraries 
### dayjs(https://day.js.org/)
dayjs 是一个轻量级的处理时间格式的第三方库，仅需要2kb，内存占用极小，同时拥有和moment相同的API，用dayjs替换项目中的moment

## 2,Cache API data useing React-Query  (Please check why useMutation will fetch api again although params not update!!!!!!!!!)
### useQuery(for get request)
```
import type {UseQueryResult} from 'react-query;
import type {ResponseData} from './types';
import {useQuery} from 'react-query';
import getInfo from './apis';

api:
export const useGetInfo = (params):
UseQueryResult<
ResponseData,Error
> => {
  return useQuery(
  [params],//param 1:string | array,one key use string,keys use array,will fetch api again when keys update
  ()=>getInfo(params);//param 2:api to fetch data
);
};

page:
import {useGetInfo} from './query';

const {data,isLoading,isError,error} = useGetInfo(params);//It will fetch api
//It can reducing use useState to store data

```

### useMutation(for post request)
```
import type {useMutationResult} from 'react-query';
import type {AxiosError} from 'axios';
import type {InputterSubmitResponse,Params} from ''./types;
import {useMutation} from 'react-query';
import {postInputterSubmit} from './apis';

//api:
export const useInputterSubmit = ():
useMutationResult<
InputterSubmitResponse,
AxiosError<APIErrorResponse>,
Params
> => {
  return useMutation(postInputterSubmit);
};

//page:
import {useInputterSubmit} from './query';

const {mutate,data,isLoading,error,isError} = useInputterSubmit;
//It can reducing use useState to store data

useEffect(
()=>{
mutate(params)//it will fetch api
},[]);

```
