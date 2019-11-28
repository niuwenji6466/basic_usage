@Qualifier注解了，qualifier的意思是合格者，通过这个标示，表明了哪个实现类才是我们所需要的，我们修改调用代码，添加@Qualifier注解，需要注意的是@Qualifier的参数名称必须为我们之前定义@Service注解的名称之一！

```
@Service("a")
public class EmployeeServiceImpl implements EmployeeService {
    public EmployeeDto getEmployeeById(Long id) {
        return new EmployeeDto();
    }
}

@Service("b")
public class EmployeeServiceImpl1 implements EmployeeService {
    public EmployeeDto getEmployeeById(Long id) {
        return new EmployeeDto();
    }
}


@Controller
@RequestMapping("/emplayee.do")
public class EmployeeInfoControl {

    @Autowired
    @Qualifier("b")
    EmployeeService employeeService;

    @RequestMapping(params = "method=showEmplayeeInfo")
    public void showEmplayeeInfo(HttpServletRequest request, HttpServletResponse response, EmployeeDto dto) {
        //略
    }
}
```



