请求相机权限，未授权过的话弹出系统弹框，不是第一次授权直接返回结果,如果被禁止了提示用户去开启，支持8.0以上系统直接跳转到设置

    + (void)requestUseVideoCamera:(void(^)(BOOL isCanUse))CompletionHandler
    {
            NSString *tipTextWhenNoPhotosAuthorization; // 提示语
            NSString *mediaType = AVMediaTypeVideo;     //读取媒体类型
            AVAuthorizationStatus authStatus = [AVCaptureDevice authorizationStatusForMediaType:mediaType];          //读取设备授权状态
            if(authStatus == AVAuthorizationStatusRestricted || authStatus == AVAuthorizationStatusDenied) {
                NSDictionary *mainInfoDictionary = [[NSBundle mainBundle] infoDictionary];
                NSString *appName = [mainInfoDictionary objectForKey:@"CFBundleDisplayName"];
                tipTextWhenNoPhotosAuthorization = [NSString stringWithFormat:@"请在\"设置-隐私-相机\"选项中，允许%@访问你的手机相机", appName];
                UIViewController *currentController = [[AppDelegate appDelegate] getNewCurrentViewController];

                [self showAlertViewFromController:currentController
                                            title:@"温馨提示"
                                          message:tipTextWhenNoPhotosAuthorization
                                CancleButtonTitle:@"取消"
                                 otherButtonTitle:@"去设置"
                                cancleButtonClick:^{

                                } otherButtonClick:^{
                                    [self openSystemSetting];
                                }];
                // 展示提示语
                NSLog(@" -- %@ ",tipTextWhenNoPhotosAuthorization);
                if (CompletionHandler) {
                    CompletionHandler(NO);
                }
            }
            else if(authStatus == AVAuthorizationStatusNotDetermined) { //第一次请求。
                [AVCaptureDevice requestAccessForMediaType:AVMediaTypeVideo completionHandler:^(BOOL granted) {
                        dispatch_async(dispatch_get_main_queue(), ^{
                            if (CompletionHandler) {
                                 CompletionHandler(granted);
                            }
                          }];
                 });
            }
            else {
                if (CompletionHandler) {
                    CompletionHandler(YES);
                }
            }
    }

    + (void)openSystemSetting
    {
        if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]]) {
            [[UIApplication sharedApplication] openURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]];
        }
    }
    + (void)showAlertViewFromController:(UIViewController *)controller
                                  title:(NSString *)title
                                message:(NSString *)message
                      CancleButtonTitle:(NSString *)cancleTitle
                       otherButtonTitle:(NSString *)otherTitle
                      cancleButtonClick:(void(^)(void))cancleClick
                       otherButtonClick:(void(^)(void))otherButtonClick
    {
        UIAlertController *alertController = [UIAlertController alertControllerWithTitle:title
                                                                                 message:message
                                                                          preferredStyle:UIAlertControllerStyleAlert];
        [alertController addAction:[UIAlertAction actionWithTitle:cancleTitle
                                                            style:UIAlertActionStyleCancel
                                                          handler:^(UIAlertAction * _Nonnull action) {
                                                              cancleClick ();
                                                   }]];          
        
        [alertController addAction:[UIAlertAction actionWithTitle:otherTitle
                                                            style:UIAlertActionStyleDefault
                                                          handler:^(UIAlertAction * _Nonnull action) {
                                                              otherButtonClick ();
                                                          }]];
        
        [controller presentViewController:alertController
                                 animated:YES
                               completion:nil];
    }

需要导入
    
    #import <AVFoundation/AVCaptureDevice.h>

其中遇到的比较坑的点是

    [AVCaptureDevice requestAccessForMediaType:AVMediaTypeVideo completionHandler:^(BOOL granted) { 
    //分线程
    });

回调的block是分线程。不小心在block里操作UI就会有诡异的现象发生，比如push会大约8秒才跳转。
