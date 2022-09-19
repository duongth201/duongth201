## JS, JQuery

1. Khai báo ```JQuery```: link từ CDN của GG, MS (có mạng, tải lâu) hoặc download file js về đính kèm (không có mạng)

- $(selector).action()

    - Các dạng selector: ("p"), ("#myId") -> trả về 1 phần tử, (".myClass") -> trả về 1 list phần tử
    
    <img src="https://i.pinimg.com/564x/76/16/83/761683f219e8d27f50f552fcf32af970.jpg">
    
- **.val()**: get hoặc set giá trị cho 1 element
- **.attr()**: get hoặc set giá trị cho 1 attribute. Nếu chưa có sẽ tự động thêm mới
- **.html()**: get hoặc set html cho element
- **.text()**: get hoặc set text cho element
- **.addClass()**
- **.removeClass()**
- **.removeAttr()**
- **.remove()**: remove element
- **.find("id, class")**: tìm element trong element cha

***
```
    <!-- Thư viện JS -->
    <script src="./js/jquery-3.6.0.min.js"></script>
    <!-- JS dùng chung -->
    <script src="./js/common.js"></script>
    <script src="./js/enum.js"></script>
    <script src="./js/resource.js"></script>
    <!-- JS dùng riêng -->
    <script src="./js/pages/page.js"></script>
```
-- **Bắt buộc** có trong js để chạy func
```
$(document).ready(function () {
    function();
})
```

```
/**
 * Tạo event cho các elements
 * Author: DuongTH (04/09/2022)
 */
function initEvent() {
    try {
        //**Hiển thị dialog** nhân viên khi nhấn button Thêm mới
        $("#btnAdd").click(function () {
            $("#dlgEmployeeDetail").show();
        })

        //Ẩn dialog khi nhấn button Hủy
        $('html').on('click', '.dialog .button--cancel', function () {
            $(this).parents(".dialog").hide();
        })
        //Ẩn dialog khi nhấn button X
        $('html').on('click', '.dialog .dialog__button--close', function () {
            //show dialog cảnh bảo
            common.showCancelDialog();
            //nhấn button Không
            $('html').on('click', '.dialog #btnCancel', function () {
                $(this).parents(".dialog").hide();
                $('#dlgEmployeeDetail').hide();
                loadData();
            })
            //Nhấn button Có
            // $(document).on('click', '#btnSave4', function () {
            //     if($('#dlgNotify3btn').hasClass('.dialog__display')){
            //         $('html').on('click', '#dlgNotify #btnClose', function () {
            //             $(this).parents(".dialog").hide();
            //             document.getElementById("noti4").style.display = "none";
            //         })
            //     } else {
            //         document.getElementById("noti4").style.display = "block";
            //     }
            //     saveData();
            // });
        })
        //Ẩn dialog cảnh báo khi bấm Đóng 
        $('html').on('click', '#dlgNotify #btnClose', function () {
            $(this).parents(".dialog").hide();
        })

        //Ẩn dialog Xóa dữ liệu khi nhấn btn Có
        $('html').on('click', '#btnYes ', function () {
            $(this).parents(".dialog").hide();
        })

        //Cất dữ liệu khi nhấn button Lưu
        $(document).on('click', '#btnSave', saveData);


        //Double click cells
        $(document).on('dblclick', '#employeeList tbody tr', function (event) {
            // Lấy id của bản ghi: 
            if (!$(event.target).hasClass("cbox") && !$(event.target).hasClass("undblclick")) {
                const id = $(this).data('id');
                fetch(`https://cukcuk.manhnv.net/api/v1/Employees/${id}`)
                    .then(res => res.json())
                    .then(res => {
                        // binding data:
                        let inputs = $("#dlgEmployeeDetail input");
                        for (const input of inputs) {
                            const propName = $(input).attr("prop-name");
                            let value = res[propName];
                            $(input).val(value);
                        }
                    })
                $("#dlgEmployeeDetail").show();
            }
        })

        //Nhấn cells giữ td
        $(document).on('click', 'table#employeeList tbody tr', function () {
            $(this).siblings('.click__td').removeClass('click__td');
            this.classList.add('click__td');
        });

        //Nhấn checkbox giữ td
        $(document).on('click', 'table#employeeList .checkbox__group input[type="checkbox"]:checked + label', function () {
            // $(this).siblings('.click__td').removeClass('click__td');
            //this.classList.add('click__td');
            $(this).addClass('click__td');
        });

        //Nhấn icon dropdown trong chức năng
        $(document).on('click', '.contextmenu .contextmenu__dropdown', function () {
            if ($(this).hasClass('choose')) {
                $(this).removeClass('choose');
            } else {
                $(this).addClass('choose');
            }
        })

        //Lấy lại dữ liệu khi nhấn Button reload
        $(document).on('click', '.icon--reload', loadData);

        //Lập trình phím tắt
        $("#dlgEmployeeDetail").keydown(function (e) {
            // Bắt hành động khi người dùng nhấn Enter
            console.log(e.keyCode);
            const keyCode = e.keyCode;
            if (keyCode == PAGEEnum.KeyCode.ENTER) {
                $("#btnSave").trigger("click");
            }
            else if (keyCode == PAGEEnum.KeyCode.ESC) {
                $('html').on('click', '.dialog .dialog__button--close', function () {
                    $(this).parents(".dialog").hide();
                })
            }
        });

    } catch (error) {
        console.log(error)
    }
}
```


## Common.js: phần dùng chung cho cả trang web
```
var common = {
    function(){}
}
```
## Enum.js: phần phím tắt
```
var PAGEEnum = {
    //giá trị phím
    KeyCode: {
        ENTER: 13,
        ESC:27
    }
}
```
## Resource.js: thông báo theo kiểu ngôn ngữ
```
var Resource = {
    ErrorValidate: {
        EmployeeCodeNotEmpty: {
            VI: "Mã nhân viên không được phép để trống",
            EN: "Employee Code is not empty"
        }
    }
}
```




    
    
    